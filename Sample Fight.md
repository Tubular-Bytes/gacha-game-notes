
This is a sample fight before implementing the global stack.

```
seed 0
Team A:
  - Thokulnak the orc warrior [sword / robe / healing]
      skills: power_strike, orc_rage, backstab, sweep, guard, healing_potion
      modifiers: hp +20, hp +15, initiative -1, attack +3, attack +0, initiative +1
  - Aldric Crane the human mage [staff / robe / fog]
      skills: arcane_bolt, second_wind, mend, chain_lightning, flurry, fog_potion
      modifiers: attack +4, hp x0.9, attack +3, initiative +1
  - Ugulnak the orc mage [dagger / plate / healing]
      skills: arcane_bolt, orc_rage, guard, backstab, flurry, healing_potion
      modifiers: attack +4, hp x0.9, hp +15, initiative -1, initiative +2, defense +5, initiative -2
Team B:
  - Durarnak the orc mage [sword / robe / fog]
      skills: arcane_bolt, orc_rage, backstab, mend, guard, fog_potion
      modifiers: attack +4, hp x0.9, hp +15, initiative -1, attack +3, attack +2, initiative +1
  - Mogogdum the orc warrior [dagger / robe / healing]
      skills: power_strike, orc_rage, flurry, mend, guard, healing_potion
      modifiers: hp +20, hp +15, initiative -1, initiative +3, initiative +1
  - Thokashbur the orc mage [dagger / plate / fog]
      skills: arcane_bolt, orc_rage, guard, backstab, flurry, fog_potion
      modifiers: attack +4, hp x0.9, hp +15, initiative -1, initiative +2, defense +5, initiative -2

=== Round 1 ===
Mogogdum (B2) uses Power Strike
  Ugulnak (A3) takes 10.5 damage (93.0 hp left)
Aldric Crane (A2) uses Mend
  Ugulnak (A3) heals 30.0 (103.5 hp)
Thokulnak (A1) uses Sweep
  Durarnak (B1) takes 9.2 damage (94.3 hp left)
  Mogogdum (B2) takes 9.2 damage (125.8 hp left)
  Thokashbur (B3) takes 7.7 damage (95.8 hp left)
Durarnak (B1) uses Arcane Bolt
  Ugulnak (A3) takes 34.2 damage (69.3 hp left)
Thokashbur (B3) uses Fog Potion
  Fog covers the field: attack x0.8 for 2 round(s)
Ugulnak (A3) uses Healing Potion
  Ugulnak (A3) heals 40.0 (103.5 hp)
=== Round 2 ===
Mogogdum (B2) uses Healing Potion
  Mogogdum (B2) heals 40.0 (135.0 hp)
Aldric Crane (A2) uses Arcane Bolt
  Mogogdum (B2) takes 24.5 damage (110.5 hp left)
Thokulnak (A1) uses Healing Potion
  Thokulnak (A1) heals 40.0 (135.0 hp)
Durarnak (B1) uses Mend
  Mogogdum (B2) heals 30.0 (135.0 hp)
Thokashbur (B3) uses Arcane Bolt
  Ugulnak (A3) takes 20.2 damage (83.3 hp left)
Ugulnak (A3) uses Flurry
  Mogogdum (B2) takes 7.0 damage (128.0 hp left)
  Mogogdum (B2) takes 7.0 damage (121.1 hp left)
  Fog fades
=== Round 3 ===
Mogogdum (B2) uses Guard
  Mogogdum (B2): defense +6 for 2 round(s)
Aldric Crane (A2) uses Chain Lightning
  Durarnak (B1) takes 10.2 damage (84.1 hp left)
  Mogogdum (B2) takes 10.2 damage (110.9 hp left)
  Thokashbur (B3) takes 10.2 damage (85.6 hp left)
Durarnak (B1) uses Backstab
  Ugulnak (A3) takes 29.0 damage (54.3 hp left)
Thokulnak (A1) uses Backstab
  Durarnak (B1) takes 22.0 damage (62.1 hp left)
Ugulnak (A3) uses Guard
  Ugulnak (A3): defense +6 for 2 round(s)
Thokashbur (B3) uses Flurry
  Ugulnak (A3) takes 3.7 damage (50.6 hp left)
  Ugulnak (A3) takes 3.7 damage (46.9 hp left)
=== Round 4 ===
Mogogdum (B2) uses Flurry
  Thokulnak (A1) takes 6.0 damage (129.0 hp left)
  Thokulnak (A1) takes 6.0 damage (123.0 hp left)
Aldric Crane (A2) uses Fog Potion
  Fog covers the field: attack x0.8 for 2 round(s)
Durarnak (B1) uses Arcane Bolt
  Thokulnak (A1) takes 27.4 damage (95.6 hp left)
Thokulnak (A1) uses Power Strike
  Durarnak (B1) takes 13.6 damage (48.5 hp left)
Ugulnak (A3) uses Orc Rage
  Ugulnak (A3): attack x1.4 for 2 round(s)
Thokashbur (B3) uses Orc Rage
  Thokashbur (B3): attack x1.4 for 2 round(s)
  Ugulnak (A3): defense +6 wears off
  Mogogdum (B2): defense +6 wears off
=== Round 5 ===
Mogogdum (B2) uses Mend
  Durarnak (B1) heals 30.0 (78.5 hp)
Aldric Crane (A2) uses Flurry
  Durarnak (B1) takes 8.9 damage (69.6 hp left)
  Mogogdum (B2) takes 8.9 damage (102.0 hp left)
Durarnak (B1) uses Fog Potion
  Fog covers the field: attack x0.8 for 2 round(s)
Thokulnak (A1) uses Orc Rage
  Thokulnak (A1): attack x1.4 for 2 round(s)
Ugulnak (A3) uses Backstab
  Mogogdum (B2) takes 21.1 damage (80.9 hp left)
Thokashbur (B3) uses Guard
  Thokashbur (B3): defense +6 for 2 round(s)
  Ugulnak (A3): attack x1.4 wears off
  Thokashbur (B3): attack x1.4 wears off
  Fog fades
=== Round 6 ===
Mogogdum (B2) uses Orc Rage
  Mogogdum (B2): attack x1.4 for 2 round(s)
Aldric Crane (A2) uses Mend
  Ugulnak (A3) heals 30.0 (76.9 hp)
Durarnak (B1) uses Mend
  Mogogdum (B2) heals 30.0 (110.9 hp)
Thokulnak (A1) uses Guard
  Thokulnak (A1): defense +6 for 2 round(s)
Ugulnak (A3) uses Flurry
  Mogogdum (B2) takes 7.0 damage (104.0 hp left)
  Mogogdum (B2) takes 7.0 damage (97.0 hp left)
Thokashbur (B3) uses Backstab
  Thokulnak (A1) takes 12.4 damage (83.2 hp left)
  Thokulnak (A1): attack x1.4 wears off
  Thokashbur (B3): defense +6 wears off
  Fog fades
=== Round 7 ===
Mogogdum (B2) uses Guard
  Mogogdum (B2): defense +6 for 2 round(s)
Aldric Crane (A2) uses Arcane Bolt
  Durarnak (B1) takes 30.6 damage (39.0 hp left)
Thokulnak (A1) uses Backstab
  Durarnak (B1) takes 22.0 damage (17.0 hp left)
Durarnak (B1) uses Arcane Bolt
  Thokulnak (A1) takes 34.2 damage (49.0 hp left)
Ugulnak (A3) uses Guard
  Ugulnak (A3): defense +6 for 2 round(s)
Thokashbur (B3) uses Flurry
  Aldric Crane (A2) takes 9.2 damage (80.8 hp left)
  Thokulnak (A1) takes 6.2 damage (42.8 hp left)
  Thokulnak (A1): defense +6 wears off
  Mogogdum (B2): attack x1.4 wears off
=== Round 8 ===
Mogogdum (B2) uses Power Strike
  Ugulnak (A3) takes 7.5 damage (69.4 hp left)
Aldric Crane (A2) uses Second Wind
  Aldric Crane (A2) heals 22.5 (90.0 hp)
Thokulnak (A1) uses Healing Potion
  Thokulnak (A1) heals 40.0 (82.8 hp)
Durarnak (B1) uses Orc Rage
  Durarnak (B1): attack x1.4 for 2 round(s)
Ugulnak (A3) uses Backstab
  Thokashbur (B3) takes 19.0 damage (66.6 hp left)
Thokashbur (B3) uses Arcane Bolt
  Ugulnak (A3) takes 25.2 damage (44.2 hp left)
  Ugulnak (A3): defense +6 wears off
  Mogogdum (B2): defense +6 wears off
=== Round 9 ===
Mogogdum (B2) uses Healing Potion
  Mogogdum (B2) heals 40.0 (135.0 hp)
Aldric Crane (A2) uses Chain Lightning
  Durarnak (B1) takes 10.2 damage (6.8 hp left)
  Mogogdum (B2) takes 10.2 damage (124.8 hp left)
  Thokashbur (B3) takes 10.2 damage (56.4 hp left)
Durarnak (B1) uses Backstab
  Ugulnak (A3) takes 44.2 damage (0.0 hp left)
Thokulnak (A1) uses Power Strike
  Thokashbur (B3) takes 15.0 damage (41.4 hp left)
Thokashbur (B3) uses Fog Potion
  Fog covers the field: attack x0.8 for 2 round(s)
Ugulnak (A3) uses Arcane Bolt
  Mogogdum (B2) takes 20.2 damage (104.6 hp left)
  Durarnak (B1): attack x1.4 wears off
=== Round 10 ===
Mogogdum (B2) uses Flurry
  Aldric Crane (A2) takes 4.4 damage (85.6 hp left)
  Thokulnak (A1) takes 4.4 damage (78.4 hp left)
Aldric Crane (A2) uses Mend
  Ugulnak (A3) heals 30.0 (30.0 hp)
Thokulnak (A1) uses Orc Rage
  Thokulnak (A1): attack x1.4 for 2 round(s)
Durarnak (B1) uses Arcane Bolt
  Thokulnak (A1) takes 27.4 damage (51.1 hp left)
Thokashbur (B3) uses Orc Rage
  Thokashbur (B3): attack x1.4 for 2 round(s)
Ugulnak (A3) uses Flurry
  Durarnak (B1) takes 7.0 damage (0.0 hp left)
  Durarnak (B1) dies
  Mogogdum (B2) takes 7.0 damage (97.7 hp left)
  Fog fades
=== Round 11 ===
Mogogdum (B2) uses Guard
  Mogogdum (B2): defense +6 for 2 round(s)
Aldric Crane (A2) uses Flurry
  Mogogdum (B2) takes 8.6 damage (89.1 hp left)
  Thokashbur (B3) takes 9.1 damage (32.3 hp left)
Thokulnak (A1) uses Backstab
  Thokashbur (B3) takes 27.4 damage (4.9 hp left)
Ugulnak (A3) uses Guard
  Ugulnak (A3): defense +6 for 2 round(s)
Thokashbur (B3) uses Guard
  Thokashbur (B3): defense +6 for 2 round(s)
  Thokulnak (A1): attack x1.4 wears off
  Thokashbur (B3): attack x1.4 wears off
=== Round 12 ===
Mogogdum (B2) uses Orc Rage
  Mogogdum (B2): attack x1.4 for 2 round(s)
Aldric Crane (A2) uses Arcane Bolt
  Mogogdum (B2) takes 30.6 damage (58.5 hp left)
Thokulnak (A1) uses Sweep
  Mogogdum (B2) takes 7.4 damage (51.1 hp left)
  Thokashbur (B3) takes 5.9 damage (0.0 hp left)
  Thokashbur (B3) dies
Ugulnak (A3) uses Healing Potion
  Ugulnak (A3) heals 40.0 (70.0 hp)
  Ugulnak (A3): defense +6 wears off
  Mogogdum (B2): defense +6 wears off
=== Round 13 ===
Mogogdum (B2) uses Mend
  Mogogdum (B2) heals 30.0 (81.1 hp)
Aldric Crane (A2) uses Chain Lightning
  Mogogdum (B2) takes 10.2 damage (70.9 hp left)
Thokulnak (A1) uses Guard
  Thokulnak (A1): defense +6 for 2 round(s)
Ugulnak (A3) uses Orc Rage
  Ugulnak (A3): attack x1.4 for 2 round(s)
  Mogogdum (B2): attack x1.4 wears off
=== Round 14 ===
Mogogdum (B2) uses Power Strike
  Thokulnak (A1) takes 10.0 damage (41.1 hp left)
Aldric Crane (A2) uses Fog Potion
  Fog covers the field: attack x0.8 for 2 round(s)
Thokulnak (A1) uses Power Strike
  Mogogdum (B2) takes 13.6 damage (57.3 hp left)
Ugulnak (A3) uses Arcane Bolt
  Mogogdum (B2) takes 28.2 damage (29.1 hp left)
  Thokulnak (A1): defense +6 wears off
  Ugulnak (A3): attack x1.4 wears off
=== Round 15 ===
Mogogdum (B2) uses Flurry
  Thokulnak (A1) takes 4.4 damage (36.7 hp left)
  Ugulnak (A3) takes 1.9 damage (68.1 hp left)
Aldric Crane (A2) uses Arcane Bolt
  Mogogdum (B2) takes 24.5 damage (4.6 hp left)
Thokulnak (A1) uses Orc Rage
  Thokulnak (A1): attack x1.4 for 2 round(s)
Ugulnak (A3) uses Flurry
  Mogogdum (B2) takes 7.0 damage (0.0 hp left)
  Mogogdum (B2) dies
Team A wins after 15 round(s)
```