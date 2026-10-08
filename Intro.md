
## Blueprint types

* character - 2 set skills from class and race, 3 skills picked from weapon/armor pool, mandatory potion skill
* class - mandatory 1 skill, optional LUA (modifiers)
* race - mandatory 1 skill, optional LUA (modifiers)
* weapon - optional 3 skills, optional LUA (modifiers)
* armor - optional 3 skills, optional LUA (modifiers)
* potion - mandatory 1 skill, mandatory LUA (behavior or modifiers)
* skill - mandatory cooldown, mandatory LUA (behavior or modifiers)

  ### Class

  * Class always adds exactly 1 skill.
  * Class can have LUA attached, adding modifier to the current character state (immunity, damage buff, etc.)
