---
id: rules
title: Summary of Rules
sidebar_label: Summary of Rules
---

On the opening turn, players deploy starting resources.

On each following turn, players study the state of the game and submit orders representing their intent to use their available resources and influence the next state of the game.

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

In this phase, movement actions from all players are processed concurrently and combat is resolved immediately as it occurs. Each movement order applied to a naval unit consumes a movement action. Each naval unit has a limited number of available movement actions each turn.

### Effects

- Resolve combat after every movement action.

### Available Orders

#### Posture

- Attack (determine kill-on-sight list)

#### Movement Orders (consumes movement action)

- Move
- Embark Land Forces
- Disembark Land Forces
- Supply Coastal Forces (Terminates movement)
- Support Coastal Forces (Terminates movement)
- Blockade (Terminates movement)

## Land Movement Phase

In this phase, combat is resolved only after all orders from all players are executed.

### Effects

- After all orders are processed, bounce marched units and resolve combat.

### Available Orders

- March
- Entrench
- Defensive Support
- Offensive Support
- Cut Support
- Invade

## Recovery Phase

### Effects

- Generals in enemy-occupied territory retreat or disband
- Equipment in enemy-occupied territory are captured or destroyed (artillery and warships-in-port as applicable)

## Supply Phase

### Effects

- Supply penalties updated by ships returning within range of a friendly port (to prevent additional penalties) or by ending the season in a friendly port (to remove existing penalties) and for infantry moving back to supplied territory, by enemy blockades or enemy infantry movement cutting off access to supplied territory, or due to warships supplying friendly forces on adjacent coastal provinces). Units removed from play if applicable

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
