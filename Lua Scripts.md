The game library is plain files: YAML for the building blocks and Lua for everything that needs logic. This page describes where Lua is used, what a script can see and do, and what it cannot.

Lua here is Lua 5.4 in a sandbox. Scripts are small: a skill is usually three or four lines.

## Where Lua is used

| Field | In | Runs | Returns | See |
|---|---|---|---|---|
| `lua` | skill | every time the skill is used in a battle | nothing; calls `deal_damage`, `heal`, `add_modifier` | [[#Skills]] |
| `condition` | a modifier | at the start of the battle and around every turn, to decide if the modifier is active | a boolean | [[#Conditions]] |
| `roll` | class, race, weapon, armor, potion | once, when a character is rolled | a table of modifiers, tags and skills | [[#Roll scripts]] |
| `name_gen` | race | once, when a character is rolled | the character's name | [[#Name generators]] |

Roll results and names are frozen into the character as plain data. A stored character never runs its roll script again, so changing a `roll` script changes new characters only.

## The sandbox

Available:

* the basic functions: `pairs`, `ipairs`, `next`, `type`, `tostring`, `tonumber`, `select`, `pcall`, `xpcall`, `error`, `assert`, `setmetatable`, `getmetatable`, `rawget`, `rawset`, `rawequal`, `rawlen`
* the `math`, `string` and `table` libraries (`table.unpack`, `string.format`, `math.floor`, `math.max`, ...)
* `rand`, the game's own random function (see below)

Not available: `os`, `io`, `debug`, `package`, `coroutine`, `utf8`, `require`, `load`, `loadstring`, `dofile`, `loadfile`, `print`, `collectgarbage`, `string.dump`, `math.random` and `math.randomseed`. A script cannot read files, reach the network, see the clock or print anything.

### Randomness

Use `rand` and nothing else. It draws from the battle's seeded generator, so the same teams, seed and library always play out the same way.

| Call | Returns |
|---|---|
| `rand()` | a float in [0, 1) |
| `rand(n)` | an integer from 1 to n |
| `rand(a, b)` | an integer from a to b, both included |

`rand(0)` or `rand(5, 1)` is an error (empty interval). To pick from a list: `list[rand(#list)]`.

### Limits

* **Instructions:** every script call may run about one million Lua instructions. A longer one (an endless loop, for example) is stopped with "script exceeded its instruction limit". The budget is reset for each call.
* **Memory:** one battle may hold at most 16 MiB of Lua memory (scripts, tables and strings together). A script that needs more fails like one that runs too long. A normal battle uses well under 1 MiB, so keep strings and tables small and this never comes up.
* **Size:** a single file in a library may be at most 1 MiB, and a whole library 2000 files.
* **Shared state:** all scripts of one battle run in the same Lua state. A variable assigned without `local` is global and stays visible to every later script in that battle. Always write `local`.

## Skills

A skill is a YAML file with a `cooldown` and a `lua` field:

```yaml
id: power_strike
name: Power Strike
description: "Hits one random enemy for 150% of your attack minus half their defense (at least 1)."
cooldown: 2
lua: |
  local t = enemies[rand(#enemies)]
  deal_damage(t, math.max(1, caster.stats.attack * 1.5 - t.stats.defense * 0.5))
```

`cooldown` is the number of rounds the skill is unavailable after use. A skill may also carry `tags` (for example `[fire]`), which are added to all damage it deals.

### What a skill can read

These globals are set before each run:

| Name | What |
|---|---|
| `caster` | the entity using the skill (`source` is the same entity) |
| `allies` | living members of the caster's team, caster included (an array) |
| `enemies` | living members of the other team (an array) |
| `round` | the current round, starting at 1 |

An entity is a table with these fields:

| Field | What |
|---|---|
| `id` | a number, unique in the battle; ids start at 0 and run across both teams |
| `name` | the character's name |
| `team` | team number, 0 or 1 |
| `hp`, `max_hp` | current and maximum health |
| `alive` | boolean |
| `stats` | a table of fully resolved stats, e.g. `attack`, `defense`, `initiative`: base values with all active modifiers applied |
| `tags` | an array of tags the character inherits from its class, race, weapon, armor and potion |
| `class`, `race`, `weapon`, `armor`, `potion` | the ids of the blueprints the character was rolled from |

The arrays are Lua arrays: `enemies[1]` is the first one, but its `id` is not 1. Dead characters are not in `allies` or `enemies`. The battle ends as soon as one team has no one left, so a skill that is used normally has someone to hit.

What a script reads is the state from before the skill. Its own earlier calls have not happened yet: `deal_damage` followed by a read of `t.hp` still shows the old health.

### What a skill can do

A skill does not change anything itself. It calls these functions, which collect actions; the game applies them in order once the script has finished.

**`deal_damage(target, amount [, tags])`**
`target` is an entity or an id. `amount` is the base damage; `tags` is an optional array such as `{ "fire" }`. Tags are merged with the skill's own. Damage modifiers of the caster (dealt) and of the target (taken) are then applied, the result is never below 0, and a dead target is skipped.

**`heal(target, amount)`**
Restores health up to the maximum. Negative amounts are treated as 0, and dead targets are skipped.

**`add_modifier{ ... }`**
Adds a modifier for a number of rounds, or permanently. See [[#Modifier tables]].

**`heal_from_damage(target, share)`**
Heals `target` by `share` times the health that the skill's damage calls have taken from their targets since the previous `heal_from_damage` (or since the skill started). The amount is the health actually lost: after the damage modifiers of both sides, and without overkill, so a hit of 100 on a target with 10 health left counts as 10. Damage to different targets all counts. Everything else works like `heal`: capped at the maximum, dead targets skipped. See [[#Healing from damage]].

A script that calls none of them does nothing. A call with an unknown target id (`deal_damage(99, 5)`) makes the whole battle fail.

### Examples

Hit everyone and apply a timed effect:

```lua
for _, t in ipairs(enemies) do
  deal_damage(t, caster.stats.attack * 0.5)
end
add_modifier{
  damage = "dealt", op = "add", value = -2,
  applies_to = { tags = { "frost" } }, duration = 3, name = "Firestorm",
}
```

Heal yourself: `heal(caster, 40)`

Raise your own defense: `add_modifier{ target = caster, stat = "defense", op = "add", value = 6, duration = 2 }`

A global effect on everyone, allies and enemies: `add_modifier{ stat = "attack", op = "mul", value = 0.8, applies_to = {}, duration = 2, name = "Fog" }`

### Healing from damage

`heal_from_damage` **uses up the damage it heals from**. Each call looks at the damage dealt since the last call, heals from it once, and then the counter starts again from zero. This means:

* `deal_damage` in a loop followed by one `heal_from_damage` heals a share of all the hits together.
* A `heal_from_damage` inside the loop, after each hit, heals the same total: every hit is counted once, in the call that follows it.
* Two calls in a row: the second one has nothing left and heals nothing. To split the damage between several targets, work out the amounts yourself and use `heal`.
* Damage that comes after the last `heal_from_damage` is not counted by anything.
* The counter belongs to one use of one skill. It starts at zero every time a skill is used and is never carried over to another skill or another turn.
* The damage is used up even when the heal does not happen (a dead target, a share of 0 or less). A call with no damage before it heals nothing and adds no line to the battle log.

```lua
-- Drain: hits one enemy and heals you for half of the health they lose
local t = enemies[rand(#enemies)]
deal_damage(t, math.max(1, caster.stats.attack - t.stats.defense * 0.5))
heal_from_damage(caster, 0.5)
```

## Modifier tables

Modifiers are data, written either in YAML (on a block) or as a Lua table (in `add_modifier` or a roll script). Exactly one of `stat` and `damage` says what changes:

```yaml
- { stat: attack, op: mul, value: 1.25, condition: "source.hp < source.max_hp * 0.5" }
- { damage: dealt, tags: [frost], op: add, value: -1, applies_to: { team: enemy }, duration: 3 }
```

| Field | Meaning |
|---|---|
| `stat` | a stat name: `hp`, `attack`, `defense`, `initiative`, or any other name |
| `damage` | `dealt` (damage the character deals) or `taken` (damage it receives) |
| `tags` | with `damage`: only damage carrying any of these tags; leave out for all damage |
| `op` | `add` or `mul` |
| `value` | the number |
| `applies_to` | who is affected, see below; by default the owner (or the caster, for `add_modifier`) |
| `condition` | a Lua condition, see [[#Conditions]] |
| `duration` | rounds the modifier lasts; leave out for permanent. It counts down at the end of each round |

All adds are summed first, then all muls are multiplied in: `(base + sum of adds) * product of muls`. A stat that only appears in modifiers starts at 0.

`add_modifier` also takes two conveniences: `target` (an entity or id, shorthand for `applies_to = { id = ... }`, not combined with `applies_to`) and `name` (the label shown in the battle log; default is the skill's name).

### Who is affected

`applies_to` is a selector. Every field you give must match; no fields means everyone.

| Field | Matches |
|---|---|
| `id` | one entity |
| `name` | the character's name |
| `team` | `ally` (includes the source) or `enemy`, relative to the source |
| `class`, `race`, `weapon`, `armor`, `potion` | characters rolled from that blueprint id |
| `tags` | characters that have all of these tags |

`applies_to = {}` is a global effect.

### Which blocks may change what

Stats are sorted into categories: `attack` is offensive, `defense` is defensive, everything else is misc. Damage dealt is offensive and damage taken is defensive. Classes, races and potions may change anything. Weapons may change offensive and misc, armor defensive and misc. This is checked for blocks and roll results when the library loads; a modifier added by a skill during a battle is not restricted.

## Conditions

A condition decides whether a modifier is active right now. It is either an expression:

```yaml
condition: "source.hp < source.max_hp * 0.5"
```

or a function body that returns a boolean:

```yaml
condition: |
  local mine = source.stats.attack
  return mine > 12 and #enemies > 1
```

It must return exactly `true` or `false`; anything else is an error. In a condition, `source` and `caster` are the character the modifier belongs to (or the one who cast it), and `allies`, `enemies` and `round` are available as in a skill.

Conditions are checked once before the first round (when `round` is 0), then at the start and at the end of every turn, and at the end of every round. So a modifier can switch on and off during the fight, and the battle log shows when it does. A condition sees the stats from the previous check, not the result of the one in progress, so a condition on `stats.attack` of a modifier that changes attack does not feed back into itself.

## Roll scripts

A `roll` script on a class, race, weapon, armor or potion runs once when a character is rolled. It can return a table, or nothing:

```lua
return {
  modifiers = {
    { stat = "initiative", op = "add", value = rand(1, 3) },
  },
}
```

| Key | Effect |
|---|---|
| `modifiers` | a list of modifier tables, added to the block's own |
| `tags` | extra tags for this character, added to the block's |
| `skills` | replaces the block's skill list for this character |

Any other key is an error. The same rules as for the block apply: a class, race and potion provide exactly one skill, a weapon or armor at most three, skills must exist and must not be retired, and the category rules of [[#Which blocks may change what]] hold.

A roll script has `rand` and the standard libraries, but no `caster`, `enemies` or other battle state, because there is no battle yet.

## Name generators

A race may have a `name_gen` script that returns the character's name as a string:

```lua
local first = { "Aldric", "Brenna", "Cedric" }
local last = { "Ashford", "Blackwood", "Crane" }
return first[rand(#first)] .. " " .. last[rand(#last)]
```

An empty name is an error. A race without `name_gen` names its characters after the race.

## When a script fails

* **A skill or condition fails during a battle** (a Lua error, an unknown target, a non-boolean condition, hitting the instruction limit): the whole battle is marked as failed and the player sees an error. This is why the checks below exist.
* **Syntax errors** are found when the library loads and keep it from loading.
* **A skill that fails when run** is found by a trial run. Before the game server switches to a new library version it runs every skill once (retired ones too) from characters rolled out of that version. A skill that errors keeps the version out and the old library stays active. The same trial runs once at startup on the loaded library, where a failure only logs a warning. It is a smoke test: a skill that only fails in a particular situation, for example when a certain enemy is dead, is not found.
* A character that was rolled earlier keeps working only as long as every blueprint and skill it uses still exists. That is why ids are retired (`retired: true`) instead of deleted.

## Good habits

* Write `local` for every variable.
* Prefer `math.max(1, ...)` over letting damage go to zero or below when a skill is meant to always do something.
* Describe what the skill does in `description`: players read it in the wiki and in tooltips, and the script is never shown to them.
* Keep the maths in the script obvious. A reader should be able to check the description against the code.
