---
title: Final Fantasy VII Restricted - Jobs analysis
---

# Applying Final Fantasy Jobs to FF VII

## Jobs to consider
Some jobs are mechanically unworkable or otherwise undesirable in FF VII:
- `Dancer`: No randomised dances. Status magic nature of the role can be provided elsewhere.
- `Chemist`: Mix and doubling recovery item potency can't be achieved.
- `Necromancer`: is essentially an undead `Blue Mage` / `Summoner`. Unnecessary, and annoying to keep zombified.
- `Cannonneer`: Random effects and Combine can't be achieved. long range can be provided elsewhere.
- `Oracle`: Random effects can't be achieved.
- `Viking`: Provoke can't be achieved.

Some jobs can be merged with some ease, leaving a final list as follows:

- `Warrior` / `Knight`
- `Mystic Knight` / `Dark Knight`
- `Monk` / `Black Belt` / `Master`
- `Thief` / `Ninja` (/ `Samurai`)
- `White Mage` / `White Wizard` / `Devout`
- `Black Mage` / `Black Wizard` / `Magus`
- `Red Mage` / `Red Wizard` / `Sage`
- `Ranger` / `Gunner`
- `Blue Mage` / `Gun Mage`
- `Dragoon`
- `Berserker`
- `Mimic`
- `Summoner` / `Evoker`
- `Time Mage`
- `Beastmaster`
- `Songstress` / `Bard`
- `Geomancer`
- `Lady Luck` / `Gambler` (/ `Samurai`)
- `Gladiator`
- `Scholar`

## Job features
### Warrior / Knight / Paladin / Gladiator
- Variety of equipment
- Cover
- High strength, stamina (HP)
- Knight (I, III), Paladin (IV): low-level white magic
- Gladiator (Va): Crit / nothing (Deathblow)

### Mystic Knight / Dark Knight / Gladiator
- Dark Knight is here due to FFIII, in which they are more a magic using knight
- Good physical and magical damage stats
- Mystic Knight (V): Elemental magic added to physical attacks
- Dark Knight (III): low-level white and black magic
- Gladiator (Va): Elemental attack

### Monk / Black Belt / Master
- Unarmed damage
- Monk (V): Multiple attacks (2 attacks per attack command)
- HP bonuses
- Kick (attack all enemies)
- Counter
- Monk (V): Chakra - heal / poison/darkness cure, target self only

This leaves the following job implementations:
## Knight
### FFV notes
The Knight's abilities are very limited, since much of the job is about being able to equip everything.

- Guard (not implementable in VII)
- Cover
- 2-handed (not implementable in VII)

### Suggested Materia loadout
- Command
    - (Deathblow) - fits the theme, but not traditional?
    - (Double-cut) - fits the theme but belongs to `Ranger`
    - (Slash-all) - fits the theme but arguably belongs to `Monk`
        - Slash-all
- Independent
    - Cover
    - (HP Plus) - fits the theme but belongs to `Monk`
    - (Enemy Lure) - fits the theme
    - (Pre-emptive) - sort of fits the theme? Maybe `Samurai` (Auron)?
    - (Counter) - fits the theme but belongs to `Monk`
    - (Mega-all) - capable of providing Slash-all?
- Support
    - (Elemental) - optional to make things interesting. Allows equipping elemental materia, but not using the magic
    - (Added Effect) - as above with status materia
    - (HP Absorb) - maybe fits the theme but non-traditional
    - (Added Cut) - fits the theme but might not have anything to pair with
    - (All) - might not have anything to pair with

## Monk
### FFV notes
- Kick (Slash-all is similar)
- Counter
- Focus (not implementable in VII for physical damage)
- Barehanded (unarmed attack power, not implementable in VII, but ok cos Tifa has a range of weapons)
- Chakra (self-heal, removes poison and darkness - could do via accessories)
- Increased HP
- Monks attack twice for each "attack" command

### Suggested Materia loadout
- Command
    - Slash-all
        - Slash-all
    - Double-cut
        - (2x only) - optional FFV restriction
- Independent
    - Counter
    - HP Plus
    - (Mega-all) - capable of providing Slash-all?
- Magic
    - (Restore) - optional FFV rules
        - Cure, target self only
    - (Heal) optional FFV rules
        - Poisona, target self only
        - Esuna, target self only, heal poison / blind only
- Support
    - (All) - might not have anything to pair with

## Thief
### FFV notes
- Find Passages (not implementable)
- Flee (not implementable)
- Sprint (not implementable, unless you REALLY want to not allow running without a Thief in the current party!)
- Steal
- Vigilance (not implementable)
- Mug

### Suggested loadout
- Command
    - Steal
- Independent
    - Speed Plus
    - Gil Plus
    - (Enemy Away) - fits the theme, perhaps in lieu of "Flee"
    - (Pre-emptive) - fits the theme (speed) but belongs to `Samurai`?
- Support
    - Steal as well - fits the theme but might not have anything to pair with
    - (Added Cut) - could validly be used to provide "Mug" before the Materia has learnt it?
    - (Sneak attack) - fits the theme but might not have anything to pair with
- Accessories
    - Sneak glove