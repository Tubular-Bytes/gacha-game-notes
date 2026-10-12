# Rolling back the library

How to undo a library change, when that is safe, and what to do instead when it is not.

## The short version

- **Prefer a forward fix.** Publish a new version that repairs the problem, with ids kept and unwanted things *retired*, never deleted. This always works.
- **Rolling back is activating an earlier stored version.** It is instant and nothing is deleted, but it is only safe if that version contains **every id the stored characters use**. A version from before players got characters with newer blocks does not.
- **Rolling back by hand needs a check afterwards**: the service can say one version is active while the game keeps running another (see [[#When the game refuses a rollback]]).

## What a version is

A library version is the SHA-256 of its files, so the same content is always the same version id, wherever and however often it is imported. Versions are immutable and stay stored in the library service; activating one just changes which is *active*. Game servers follow the service's active version and check it against the stored characters before they switch (the *activation guard*).

Each tag in the library repository (`v1.0.0`, `v2.0.0`, ...) is a state you can publish again. Publishing a tag whose content is already stored reuses that version (the workflow reports `new: false`).

## Where to look

| Question | Where |
|---|---|
| Which versions are stored, which is active (per the service)? | The library service's list of versions. |
| Which version does the game actually run? | The game's own health information (`library_version`), or the `version` in the wiki data it serves. |
| Did the game refuse a version? | The game server's log: look for `library rejected` and `library activated`. |
| Staging | The same, on the staging environment. |

The service's *active* version and the game's *running* version should be equal a few seconds after an activation. If they are not, the game refused the new one.

## Rolling back

1. **Choose the target.** A tag whose content you published before, or a stored version id. Check that it has every id in use (next section).
2. **Roll back through the workflow**: run *publish-library* with `environment`, `ref` set to the target tag and `activate` on. The run waits until the game reports that version and fails, saying so, if it does not.
3. **Or activate by hand** (the workflow cannot name a version id): call the library service's activation URL for that version, with the admin token. The token is kept by the maintainer and is never pasted into logs or chat.
4. **Verify** the game runs it (table above). Do not stop at the service's answer.

Game servers pick a change up within `library_poll_seconds` (10 by default), with no restart. Requests in flight finish with the library they started with.

## Is the target safe?

The guard looks at the stored teams. The target must still contain every class, race, weapon, armor, potion and skill id that any stored character uses. Retired ids count as present.

| Rolling back to... | Safe? |
|---|---|
| A version that only changed numbers, Lua, cooldowns or texts | Yes. Skill edits apply to every character at once. Block edits only ever affected characters rolled afterwards. |
| A version that has all ids players have, some retired | Yes |
| A version from before ids players now use existed | **No**: the game refuses it (below) |
| A version that deletes an id instead of retiring it | **No**, the same |
| Forward again to a newer version that is a superset | Yes |

Going *forward* to a version that only adds things is always safe, which is why a new version beats an old one.

## When the game refuses a rollback

Example: `v2.0.0` added races, players rolled characters with them, and you activate `v1.0.0`, which has none of those races.

1. **The service accepts it.** The library service checks that the library is valid; it cannot see characters. `GET /api/library/current` now says `v1.0.0`.
2. **The game refuses it and keeps running `v2.0.0`.** The guard fails, and the server logs `library rejected` with the missing ids and how many characters use them, for example `race 'bat' (used by 12 characters)`. Players notice nothing. The server remembers the rejected version and does not try it again until the service's active version changes.
3. **The two now disagree.** Service: `v1.0.0`. Game: `v2.0.0`. The workflow's wait step fails after three minutes with "the game still reports ...".
4. **The next restart makes it real.** A restarted server loads whatever the service calls active and runs no guard at that moment; it only logs a warning (`the library does not match the stored characters`). A pod restart, a deploy or a new replica puts the game on `v1.0.0`. Then:
   - a team with a character whose ids are missing cannot start a battle: the request is refused with `409` and a message naming the character and what is missing;
   - battles already queued for such a team fail;
   - nothing is lost: characters, teams and past battles are untouched, and past battles keep their log and team snapshots.

**Recovery:** activate the version the game was running (or a later one that has all the ids) with the workflow or by hand, then verify. If the game was restarted into the old version, the next poll switches it back to the active one, since going to a superset passes the guard.

Never leave the service on a version the game refused. Fix it right away, before something restarts.

## The forward-fix strategy

Cut a new version that fixes the problem and keeps every id. For example, `v2.0.1` after a bad `v2.0.0`.

- **Retire, do not delete.** Set `retired: true` on a block or skill you no longer want. A retired block is no longer rolled and a retired skill can no longer be newly picked, but existing characters keep both and keep fighting.
- **Retire the blocks that offer a retired skill** in the same change (an active block may not offer one), or swap the skill. Roll scripts may not return retired skills either.
- **Fix behaviour in skills.** A skill's Lua and cooldown are looked up by id whenever a battle runs, so editing a skill repairs it for every character at once.
- **Blocks cannot repair existing characters.** A character keeps the frozen modifiers and skills it was rolled with. If a block was wrong, the fix is to retire it so nobody else gets it. To change what existing characters do, change the skills they carry.
- **No renames or migrations.** To rename something, retire the old id and add a new one.
- **Name and tag it** like any release (`v2.0.1`), publish it to staging first, then production. The result is a version no rollback is needed for.

## Before you release: make it rehearsable

1. Publish the tag to **staging** with `activate` off, read the summary, then with `activate` on.
2. On staging, create characters with the new blocks and fight. This is also what makes staging a rollback rehearsal: try the rollback there and read the game's log, as above, before production ever depends on it.
3. Publish the same tag to production with `activate` off first, check the summary (version, commit, file count, `new`), then activate.

If you cannot tell whether a version is a safe rollback target, import it without activating and look at its file list in the service, or try it on staging.

