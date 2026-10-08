
## Blueprint types

* character - 2 set skills from class and race, 3 skills picked from weapon/armor pool, mandatory potion skill
* class - mandatory 1 skill, optional LUA (modifiers)
* race - mandatory 1 skill, optional LUA (modifiers)
* weapon - optional 3 skills, optional LUA (modifiers)
* armor - optional 3 skills, optional LUA (modifiers)
* potion - mandatory 1 skill, mandatory LUA (behavior or modifiers)
* skill - mandatory cooldown, mandatory LUA (behavior or modifiers)

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