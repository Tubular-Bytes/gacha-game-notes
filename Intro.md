
## Blueprint types

* character - 2 set skills from class and race, 3 skills picked from weapon/armor pool, mandatory potion skill
* class - mandatory 1 skill, optional LUA (modifiers)
* race - mandatory 1 skill, optional LUA (modifiers)
* weapon - optional 3 skills, optional LUA (modifiers)
* armor - optional 3 skills, optional LUA (modifiers)
* potion - mandatory 1 skill, mandatory LUA (behavior or modifiers)
* skill - mandatory cooldown, mandatory LUA (behavior or modifiers)

  ### Character

  A character is made up of building blocks that are the entities below. A character blueprint has only the default values of resources which upon instantiating get every other blocks randomly assigned (thus giving the game the gacha feel)

  Player agency is limited. They can pick the 3 skills from the weapon/armor pool to shape their characters.

  (For future reference, explore possibility of setting priority for skills)

  ### Class / Race

  * Class and race always adds exactly 1 skill.
  * Class and race can have LUA attached, adding modifier to the current character state (any type of modifier)

  ### Weapon / Armor

  * Weapon and armor offer up to 3 skills
  * Weapon and armor can have LUA attached, adding modifier to the current character state (offensive or misc type for weapon, defensive or misc type for armor)

  ### Potion

  * Potion gives a mandatory skill on top of the 5 skills (2 class/racial + 3 weapon/armor) the character has.
  * They always have a cooldown
  * It’s mandatory to attach LUA to it to describe its behavior

  ### Skill

  * A skill is anything that interacts with the game.
  * They always have a cooldown
  * It’s mandatory to attach LUA to it to describe behavior
  * Can affect self, allies, enemies or add a global effect
  * Skills are greedy. Whenever a skill is available it will be triggered as long as no other skill has been triggered in the same turn. If there are more than one skills off cooldown, the skill to use will be picked randomly.



```
id: dagger
name: Dagger
skills: \[backstab, flurry\]
roll: |
  return {
    modifiers = {
      { stat = "initiative", op = "add", value = rand(1, 3) },
    },
  }
```