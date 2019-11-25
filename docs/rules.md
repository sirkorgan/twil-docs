---
id: rules
title: Summary of Rules
sidebar_label: Summary of Rules
---

On the opening turn, players deploy starting resources.

On each following turn, players study the state of the game and submit orders representing their intent to use their available resources and influence the next state of the game.

Orders can be submitted either in text form using a special notation, or (later) via the graphical web UI.

When all orders are submitted for a turn, the orders will be applied and the state of the next turn will be resolved according to the rules. After orders are applied and rules are resolved, the next turn begins.

## Anatomy of Turn Resolution

Each turn is resolved in phases, described in the sections below.

Orders for each phase are given in advance of turn resolution. It is possible that some orders will be thwarted during the course of turn resolution; players who anticpate future events will issue the most effective orders.

The requirements listed for a phase must be satisfied by the given orders **before** the turn is resolved. Orders may not be submitted unless they ensure that all requirements of all phases are satisfied.

The effects listed for a phase will occur before processing the orders associated with that phase unless otherwise noted.

## New Game Phase

This phase only occurs at the beginning of the first turn.

### Requirements

- Must deploy all starting resources

### Available Orders

- Deploy

## Naval Movement Phase

Each movement order applied to a naval unit consumes a movement action. Each naval unit has a limited number of available movement actions each turn.

### Effects

- Warships in port may not take part in combat, and cannot be ordered to support, supply or blockade.

### Available Orders

#### Posture

- Attack (determine kill-on-sight list)

#### Movement Orders (consumes movement action)

Each round of movement orders for all players will be executed in the following order:

- Transfer of Control
- Move
- Disembark Land Forces
  - Free if ship in port
  - If not in port, disembarking forces' movement actions set to zero
- Embark Land Forces
  - Free if ship in port
- Supply Coastal Forces (Sets remaining movement actions to zero)
- Support Coastal Forces (Sets remaining movement actions to zero)
- Blockade (Sets remaining movement actions to zero)
- Salvage (Sets remaining movement actions to zero)

### Automation

#### Order Processing

```
execute all transfer of control orders
cancel all orders to foreign units where the foreign nation has not authorized transfer of control

resolve submarine combat

organize movements into rounds
for each rount of movement {
  execute each movement
  for each location with hostile forces {
    mark combats
    for each unit in location {
      cancel remaining movement orders
    }
  }
}

for each combat {
  resolve combats in order:
    - between mutually targeted boats
    - unilaterally targeted hostilities
    - mutually untargeted hostile boats
    - unilaterally untargeted hostilities
}

for each remaining order {
  if ordered unit is alive and is in the correct location {
    execute order
  }
}
```

#### Naval Combat Resolution

```
TODO:
```

## Land Movement Phase

Rail Movement must be ordered before any other order given to the land unit. At tech levels 1-3, Rail Movement allows a unit movement across up to 3 provinces along the railroad prior to the unit taking its action. At tech level 4, units can travel across 4 provinces. At level 5, units can travel across 5 provinces.

### Effects

- Units disembarking from a ship not in port may not move this turn (but may be ordered to march or invade at their disembarked position).

### Available Orders

- Transfer of Control
- Rail Movement
- March
- Entrench
- Defensive Support
- Offensive Support
- Cut Support
- Invade

### Automation

#### Order Execution

```
execute all transfer of control orders
cancel all orders to foreign units where the foreign nation has not authorized transfer of control

organize rail movements into rounds
for each round of rail movement {
  execute each movement
  for each location with hostile forces {
    mark combats
    for each unit in location {
      cancel remaining unit orders
    }
  }
}

execute all remaining orders

for each neutral unoccupied march destination {
  if there is no tie for the largest force {
    bounce all smaller forces
  } else {
    bounce all forces
  }
}

for each invasion {
  if destination was neutral and unoccupied {
    bounce all forces which entered the destination location by march order
  }
  resolve combats in order:
    - between invader and targeted forces
    - between invader and invading forces
    - between invader and occupying forces

  for each force in each combat {
    if force has a general {
      if tech level 1 {
        roll 1d6
        if result is even {
          reroll worst losing combat result at most once this season
        }
      } else if tech level 2 {
        reroll worst losing combat result at most once this season
      } else if tech level 3 {
        reroll worst losing combat result at most twice this season
      } else if tech level 4 {
        reroll each losing combat result once
      } else if tech level 5 {
        reroll each losing combat result at most twice
      }
    }
  }
}

apply all casualties assigned during combat resolution

for each invade destination {
  if any hostile forces are still present {
    bounce all invading forces
  }
}
```

#### Land Combat Resolution

```
let defensive modifier = 0
let offensive modifier = 0

if at least one defending unit is entrenched {
  increase defensive modifier by 1
  if winter penalty applies to location {
    increase defensive modifier by 1
  }
}

increase modifiers based on tech level for each participating artillery:
  - tech level 1: offense +1, defense +1, max x2 per province
  - tech level 2: offense +1, defense +2, max x2 per province
  - tech level 3: offense +1, defense +2, max x2 per province
  - tech level 4: offense +2, defense +2, max x3 per province
  - tech level 5: offense +3, defense +3, no limit

if a general is attached to a force:
  - tech level 3: offense +1, defense +1
  - tech level 4: offense +1, defense +3
  - tech level 5: offense +1d6, defense +1d6

adjust offensive modifier by -3 if invading from warship

increase modifier by 1/2 count of tech 1 warships, rounded up, unless warships have already been used as support this turn

for each supporting tech 2+ warship {
  if warship has not already modified a combat this turn {
    increase modifier by tech level
  }
}

increase defensive modifier by 1/2 count of infantry offering defensive support, rounded up

adjust modifier by unit supply penalty (up to -3)

increase defensive modifier based on terrain:
  - if hills, +1
  - if jungle, +2 and cut all support
  - if mountains, +3

for each location participating in attack (main and support) {
  attacker receives +1
}

for each location participating in attack this turn (from all players) {
  defender receives -1
}

determine infantry dice:
  - tech level 1: 1d4 per 2 infantry
  - tech level 2: 1d6 per 2 infantry
  - tech level 3: 1d6 per infantry
  - tech level 4: 2d6 per infantry
  - tech level 5: 2d6 (+3 if defending) per infantry

roll the attacker dice and add offensive modifier to each die result
roll the defender dice and add defensive modifier to each die result

Take the difference between:
  - the sum of the modified attacking dice and
  - the sum of the modified defending dice.

If the difference zero or is in favour of the defender:
  - then the attacker loses and is assigned one casualty
  - otherwise the defender loses is assigned one casualty.

If the difference meets or exceeds 10:
  - the losing force receives one additional casualty for each multiple of 10 in the difference (14 = 1 casualty, 22 = 2 casualties, etc.)

for each force offering offensive support {
  resolve an attack from the supporting force against the defender
}
```

## Recovery Phase

Regarding artillery/ship capture: If capturing nation's tech level is lower than captured artillery/ship's tech level, the captured artillery/ship may be used for up to 1 year, after which the unit will be removed from play and half the cost of the unit refunded to the captor.

### Effects

- Generals in enemy-occupied territory retreat or disband
- Equipment in enemy-occupied territory are captured or destroyed (artillery and warships-in-port as applicable)
- Remove expired high-tech artillery/ships from play

### Automation

```
if enemy artillery or warships-in-port are present in occupied province {
  for each unit {
    roll 1d6
    if result is even {
      unit is destroyed
    } else {
      unit is captured
    }
  }
}

```

## Supply Phase

### Effects

- Supply penalties updated by ships returning within range of a friendly port (to prevent additional penalties) or by ending the season in a friendly port (to remove existing penalties) and for infantry moving back to supplied territory, by enemy blockades or enemy infantry movement cutting off access to supplied territory, or due to warships supplying friendly forces on adjacent coastal provinces). Units removed from play if applicable

### Automation

```
for each land unit {
  add one supply penalty if:
    - unit is aboard a warship
    - unit is not within or directly adjacent to fully supplied province
    - unit is cut off from

  if unit has 4 penalties {
    unit is disbanded
  }
}

for each ship {
  add one supply penalty if:
    - unit is out of range of any friendly unblockaded port

  if ship has 4 penalties {
    ship becomes abandoned
  }
}

for each abandoned ship {
  add 1 tick

  if ship has 4 ticks {
    remove ship from play
  }
}
```

## Progress Phase

### Effects

- Tech progression takes effect, placement of incoming reactionary rebels is determined
- Revolts break out in provinces being annexed (during Summer only for nationalists, or during Summer and Winter for reactionary rebels)
- Ownership of provinces updated (change in control/occupation status, if any)

## Administration Phase

### Requirements

- If no Capital controlled, must establish provisional capital

### Available Orders

- Establish Provisional Capital
- Change Leadership

## Economic Phase

### Requirements

- If Winter, must pay upkeep for all units
- If outgoing payments are scheduled, they must be authorized or canceled

### Effects

- Collect scheduled payments from other nations
- If Summer or Winter, collect income from provinces

### Available Orders

- Disband Military Units
- Disband Conscripts
- Muster Infantry
- Muster Conscripts
- Manufacture Artillery
- Manufacture Warship
- Manufacture Battleship
- Manufacture Submarine
- Construct Port
- Upgrade Port
- Construct Factory
- Construct Military Academy
- Construct Fort
- Mothball Fort
- Reactivate Fort
- Construct Railroad
- Construct Canal
- Invest in Canal
- Upgrade Tech Level
- Institute Programs to Assist Displaced Workers
- Upgrade Province
- Authorize Scheduled Payment
- Cancel Scheduled Payment

## Production Phase

### Effects

- Advance build/training progress
- New units and buildings are placed if production is complete

## Diplomacy Phase

Public Treaties are registered within the game systems and any material expectations arising from the treaty are automatically tracked. Treaties are offered via the Draft order and take effect after all named parties issue an order to Ratify the treaty. Both the drafting and ratification of a public treaty are publicly announced, but only the parties of the treaty have access to full tracking data.

Diplomacy is officially tracked via measurable Promises and Expectations.

Any under-the-table deals between players can also be tracked using Private Agreements.

### Effects

- After all diplomatic orders from all players are executed, a diplomatic report will be generated and made available to each player.

### Available Orders

- Draft Public Treaty
- Ratify Public Treaty (automatically set Expectations based on terms of the treaty)
- Record Private Agreement
- Expunge Private Agreement

## Mission Completion Phase

### Effects

- Missions completed and rewards issued
