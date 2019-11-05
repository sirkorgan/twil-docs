---
id: goldenage
title: Notes from Golden Age 1861 Rules
sidebar_label: Golden Age 1861 Notes
---

See rulebook here: https://drive.google.com/file/d/1-i4dOf869BljCSbp8YhmE87uYMlFX2Lz/view

## Process of Turn Resolution

From rulebook, with changes and additions for clarity, and start-of-next-turn things moved to end-of-this-turn.

When all orders are submitted and moderator triggers turn resolution, the following occurs in order:

- Pay military upkeep (at start of Spring). Military upkeep is involuntarily deducted from your income. You cannot opt not to pay your military. If you cannot make the payment, you will disband units as necessary to break even.
- Disband unpaid military units. The players orders will not be processed unless the required disband orders are present and valid.
- Naval embarkment and disembarkment of troops in port
- Naval movement by units which exert a Zone of Control or are immune to it
- Other naval movement
- Naval combat resolved. Victorious naval units may embark, disembark or supply as ordered if no additional movement is requirement.
- Land movement along railroads
- Land actions and movement by foot or landship (marching and invading)
- Land combat is resolved
- Generals in enemy-occupied territory retreat or disband
- Equipment in enemy-occupied territory are captured or destroyed (artillery and warships-in-port as applicable)
- Supply penalties updated by ships returning within range of a friendly port (to prevent additional penalties) or by ending the season in a friendly port (to remove existing penalties) and for infantry moving back to supplied territory, by enemy blockades or enemy infantry movement cutting off access to supplied territory, or due to warships aupplying friendly forces on adjacent coastal provinces). Units removed from play if applicable
- Tech progression takes effect, placement of incoming reactionary rebels is determined
- Revolts break out in provinces bein annexed (during Summer only for nationalists, or during Summer and Winter for reactionary rebels)
- Ownership of provinces updated (change in control/occupation status, if any)
- New units and buildings placed
- Missions complete and rewards issued, as players inform the administrator
- New map published
- Collect income (end of Winter/Summer)(logged as start of Spring/Autumn)
- Advance to next Season

## Nations

- ID
- Name
- Played by: player ID
- Current leader name
- Owned provinces
- Owned units
- Owned buildings
- Owned canals
- Owned straits
- Currency
- Tech level: 1..5

#### Further reading on Currency

- Tech level effect on economy, page 7

## Tiles / Locations

- ID
- Type: land/province, sea
- Name
- Buildings: list
- Units: list

### Province

- Neutral?
- Owned by: nation owning this tile
- Controlled by: nation exerting dominant influence on this tile (see p.16, affects prestige/missions)
- Capital? (**how to classify capital under foreign control?**)
- Overseas? (affects reactionary revolts. may change with owner. see p.15. See also p.18 for special rules for the Ottoman Empire.)
- Deep Africa? (special class of provinces that have extra requirements to colonize, see p.19-20)
- Occupied?
- Occupation-timer-ID (timer will have relevant event info, like occupier id)
- Base Value
- Upgraded? (increase base value by 1)
- Economic Value (derived; "base + factories + railroads")
- Annexed?
- Nationalist Revolt? (may be derived by: occupation by nationalist rebels, see p.17)
- Puppeted?
- Reactionary Revolt/Unrest? (roll for revolt each time revenue is collected, see p.18)
- Northern? (causes attack penalty during Winter)
- Controls strait?
- Controls canal?

### Sea zone

- Strait?
- Canal?
- Open?

### Further reading

- Occupation, page 7-8
- Blockades, straits, and canals, page 12
- Neutral provinces, puppet states, annexation, and diplomatic integration, page 13

## Units

- ID
- Land?
- Sea?
- Type: Conscript, Infantry, General, Artillery, Landship, Warship, Submarine, Nationalist Rebel etc
- Tech level: 1..5
- Classification: derived from Type and Tech level, ex. "Steam-powered Ironclad"
- Name: given/generated
- Supply penalty count
  **See pages 20-26 for specific unit information.**

### Land Unit

- Connected to capital?
- Embarkment capacity requirement: (1 for everything except Landships which require 2)
- Embarked Fleet?
- Embarked fleet ID(s)
- Embarked Rail?

### Sea Unit

- Infantry capacity: 1 for all units except battleships which have 2 (Infantry take 1 capacity, and Landships take 2. Fleet capacity can be combined by issuing the embark order to multiple naval units.)
- Support capacity: 2 (Artillery and Generals each take up 1 support capacity)
- Max Movement Actions: 10..18 (see Naval unit descriptions, page 23-26)
- Supply Range: (equal to max movement actions)
  Zone of control: 0 for all ships except Battleships, which have 1

### Further reading

- Supply, page 8

## Buildings

- ID
- Name: given/generated
- Location: Tile ID
- Type: Capital, Factory, Port, Academy, Fort
- Production timer list

Hooks: validate, pre, post. For example, building a canal. Keep logic for all construction phases and events in the related building module (**later edit: buliding spawns custom timer with related logic included?**). See page 13.

**See pages 26-31 for specific building information.**

### Capital

- See page 26-27.

### Provisional Capital

### Port

- Level?
- Blockaded?
- See page 27-28

### Factory

- See page 28.

### Fort

### Further reading

- Blockades, straits, and canals, page 12. **Note:** a sea/passive blockade disconnects a tile from the capital, but a port/active blockade targets the port specifically

## Orders (General)

The rule book can be accessed at the following location: https://drive.google.com/file/d/1-i4dOf869BljCSbp8YhmE87uYMlFX2Lz/view

### Meta

These orders are directives which apply only to the use of other orders, and do not exist as "real" orders in the system, since they do not transform the turn state.

### As

The `as` order specifies the nation that will execute all subsequent orders.

```
as "Brazil"
move ...
move ...
```

## Orders - Game Setup

### Deploy

The `deploy` order places a Nation's starting resource on the map, and is only available on Turn 0. Available starting resources differ by Nation. See page 4 for restrictions.

```
deploy {resource} {location}
```

## Orders - Military (Land)

Orders pertaining to military units, primarily Infantry. Each unit in a province receives orders individually.

### March

Non-hostile movement. Infantry enter a neutral or friendly tile and do not initiate combat, but may still have combat initiated upon them. A march order into hostile territory will be converted automatically into an invasion order.

- Marching units may use Railroads to pass through multiple provinces during a single turn. All rail movement is resolved prior to any foot movement.
- This order is also applicable to the following units: Artillery, General, Landship.
- Infantry may optionally include intent to diplomatically integrate the target Neutral province. Costs a lot of money, and results in Annexation after 3 years. See pages 13-14.

### Invade

Hostile movement. Infantry enter a tile and initiate combat.

- Optionally includes intent to annex or puppet on successful occupation. Post-occupation intent may be overwritten by reissuing the invasion order during the occupation.
- Optionally include which forces are being targeted. If the target is omitted, then two attackers simultaneously invading a tile containing existing forces belong to a third party, the two invaders will fight each other and the third party will not participate unless it was targeted by the other force. **For the online version, the engagement target should probably be required.**
- Attacking an enemy force prevents that force from providing any support during that turn.
- Occupying a province containing enemy Artillery or General units allows the occupying force to roll for the chance to capture the Artillery units or kill the General.

#### Invading the Capital

When invaded, the province containing a Nation's capital will be defended by any military forces present, as well as a special Garrison.

- Number of garrison units is equal to the economic value of the province.
- Strength of garrison units is equal to Infantry at tech levels 1 and 2, and equal to Conscripts at tech levels 3, 4, and 5.
- Garrison will always Entrench automatically and cannot be given other orders.
- Garrison, in addition to any other defending forces, must be defeated before invading forces can being occupying the province.
- Whenever the current capital or provisional capital is occupied, one half of the defending Nation's reserve currency is lost, and the next seasonal income is reduced by half.
- If the capital or current provisional capital is occupied, the defending Nation must immediately order the establishment of a new provisional capital at a valid site.
- If the defending Nation is currently using a provisional capital, and has recaptured the original capital, it may choose which capital to use as the permanent capital from that moment forwards.

#### Land Combat Mechanics

See page 32.

### Entrench

Infantry gain a defensive bonus in combat. If no orders are given to Infantry, they will entrench automatically.

### Offensive Support

Infantry supports an invasion order executed by another unit by performing a supporting attack on forces in an adjacent province, without moving. The support order must specify both the target province, and the particular forces which are being supported.

- If the supported forces do not invade, no supporting attack occurs. **I recommend that in the case the offensive support is ordered on the assumption of an invasion that was not actually ordered, the support attack goes through at a disadvantage. This is a diplomatic skill check requiring either some minimum organizational skill, or a matter of trust between uncertain allies.**
- If the supporting Infantry are engaged by another force, the support order is blocked.

### Defensive Support

Infantry support targeted forces in an adjacent province. Targeted forces receive a positive combat modifier during any engagements that turn.

- If the supporting Infantry are engaged by another force, the support order is blocked.

### Cut Support

Infantry blocks any support offered by the adjacent target forces. If the Infantry ordered to cut support numbers less than half of the number of the target force, 1d6 is rolled. On a result of 1-2, one of the Infantry units ordered to cut support is disbanded.

### Disband Military Units

Occurs gradually.

### Disband Conscripts

Can occur immediately when the Conscript is located in any core province.

### Establish Provisional Capital

Must be the first order issued on the turn following the occupation of a Nation's current capital.

### Further Reading

- See page 20-21 for Infantry build costs and detailed order descriptions, including march/invade bounce rules.

## Orders - Military (Naval)

### Move

Naval unit moves one or more sea zones from its current position. Each zone traveled consumes one movement action from the total pool of availble movement actions. Total movement actions differ by naval unit.

- The movement path must be specified exactly. Each step in the path consumes a movement action and may result in naval combat. See also the Attack order.
- Ships must spend a movement action to enter a friendly Port prior to embarking/disembarking land forces or else the embarking/disembarking forces will be penalized. See embarking/disembarking orders.
- If a ship ends the turn outside of the range of a friendly Port, it will receive a supply penalty. After the 4th supply penalty, the ship will be left abandoned at sea and becomes available to be recovered by other ships (belonging to any Nation) which end the turn in the same sea zone. See page 26.

### Attack

Ships receiving the order take on an aggressive posture toward the ships of target Nations. The attack order specifies which (or any) Nations, other than Nations it is already at war with, to be attacked on sight whenever both the ordered ships and the target Nations' ships are present in the same sea zone after a movement action.

#### Naval Combat Mechanics

- Fleets fight three engagements. The fleet which wins at least two engagements is the victor. The remaining ships of the losing fleet automatically retreat one zone towards the nearest friendly port, or enters a friendly if one is adjacent to the location of the battle.
- The winning fleet remains pinned but may carry out any Supply, Support, or Blockade orders which were intended to be performed from the sea zone in which combat occured.

### Supply Coastal Forces

Warship supplies one Infantry unit in a targeted adjacent coastal province.

- This order consumes one movement action.
- No additional movement of the ordered unit may occur after issuing this order.
- Infantry unit must be present at the target province at the end of the season in order to receive the offered supplies.

### Support Coastal Forces

Warship supports a target adjacent land force in a single engagement.

- This order consumes one movement action.
- No additional movement of the ordered unit may occur after issuing this order.
- See page 25.

### Blockade

Warship blockades the current sea zone, or a target Port located within the sea zone. Blockading a port requires more ships than blockading the sea zone, but allows the blockading force to deprive the blockaded port of seasonal income.

- This order consumes one movement action.
- No additional movement of the ordered unit may occur after issuing this order.
- See page 25.

### Embark Land Forces

Warship targets adjacent land forces up to its maximum remaining embarkment capacity. Embarked forces will automatically move with the Warship, receive supply penalties for each turn that ends while they are aboard, and may not receive orders until they are disembarked.

- This order consumes one movement action.
- If the embarked units are located in a coastal province without a friendly Port, or the embarking naval unit has not entered the Port, the embarked forces may not receive orders that turn regardless of whether or not they are subsequently disembarked at a Port.

### Disembark Land Forces

Warship disembarks selected embarked land forces to a target adjacent coastal province. The movement of the disembarked units must be declared as each a March or an Invasion.

- This order consumes one movement action.
- If the target adjacent coastal province does not contain a friendly Port, or the disembarking naval unit has not entered the Port, the disembarked forces will land but may not receive orders and will suffer a -3 penalty to each die roll in combat until the turn ends.

## Orders - Economic

Orders pertaining to economic investment and national infrastructure.

### Muster Infantry

Infantry are mustered from a Nation's capital or Military Academies. Cost and production time varies with tech level. See page 20.

### Muster Conscripts

Conscripts are mustered from any core province. Production varies with tech level and economic value of the host province. See page 22.

### Construct Artillery

Artillery is built by a Factory. A Factory can produce only one Artillery at a time.

### Construct Warship

Naval units are constructed from a Port, and when completed will be placed on the map at the Port from which they were constructed. See pages 23-26 for Naval unit information.

### Construct Battleship

Requires Level 4 Port. Limit of 2 per Nation. See page 24.

### Construct Submarine

### Construct Port

Begin construction of a Port in the target province, connected to the target adjacent sea zone.

- Only 1 port may be built per province, even if the province borders multiple sea zones.
- A port in an occupied province will not provide income.
- A port in an occupied province can be used as a supply connection for the occupying force, but will not supply the adjacent provinces.
- See page 27 for more details.

### Upgrade Port

Page 27

### Construct Factory

Begin construction of a Factory in the target province.

- Number of Factories that can be built in each province is limited to (province economic value, divided by 3 and rounded up).
- See page 28.

### Construct Military Academy

### Construct Fort

Begin construction of a Fort in the target province.

- Provides exceptional defensive bonuses to defending forces.
- Construction requires at least one Entrenched Infantry present in the province until the the Fort is complete. If this condition is violated, the construction is canceled and half the cost is refunded.
- Limit 1 Fort per province.
- Requires upkeep to be paid each Spring.
- If a province with a Fort is occupied by an enemy, the Fort will be mothballed immediately and may only be reactivated when the province is fully controlled.
- See page 29.

### Mothball Fort

The target fort will no longer require upkeep payments, but will no longer provide defensive bonuses.

### Reactivate Fort

The target fort will be reactivated over the course of two seasons while at least one Infantry is Entrenched in the province.

- Reactivation costs half the construction cost of the Fort.

### Construct Railroad

Begin construction of a railroad connecting two target provinces which are adjacent to each other.

- Number of simultaneous railroad construction projects possible is equal to the tech level of a Nation.
- At tech levels 1-3, railroads provide +3 movement to land units along rail connections. At tech level 4, the movement bonus is +4. At tech level 5, the movement bonus is +5.
- Railroads may be used to enter a province occupied by an enemy.
- Railroads remain the property of the Nation which built them, until another Nation fully controls both provinces connected by a railroad.
- Railroads increase the economic value of a province by +1 per railroad attached to that province.

### Construct Canal

`build-canal` begins construction of a building or canal. See page 12 for more on canals.

### Canal Investment

`invest-canal` spends up to 50 currency per year in a canal which is under construction. See page 12 for more on canals.

### Upgrade Tech Level

The `upgrade-tech` order spends money to increase the tech level of the nation. Reactionary revolts may be triggered as a result of the upgrade. All units and buildings will be upgraded to match the new tech level. See page 5 for details.

### Institute Programs to Assist Displaced Workers

Calms reactionary unrest if performed before a tech revolt occurs. See p.18.

### Upgrade Province

`upgrade-province` increases the land value of a province by 1, costs 10 currency, can only be done once per province, and takes effect one year after the currency is spent. See page 7.

### Authorize Scheduled Payment

Authorize a scheduled payment, causing the money to be transferred during turn resolution.

## Orders - Diplomatic

Orders pertaining to the nation's relationships with other nations, including trade.

### Loan Agreement

### Trade Agreement

See restrictions on p.16.

## Administrative

Orders pertaining to the internal management of the nation.

### Change Province Name

### Change Leader Name ('Succession')

### Move Capital

During peacetime only, the Capital building moves to target (sovereign peaceful core province controlled by and connected to existing capital) 4 turns after the order is issued, and costs (50 currency multiplied by tech level) at the end of those 4 turns.

- If the peacetime or target conditions are violated during one of turns leading up to the move, the order is canceled.

### Establish Permanent Capital

While using a provisional capital, and after recapturing an occupied original capital, a Nation may choose an existing provisional or original capital to promote to a permanent capital.

- Full seasonal income is restored when a permanent capital is in use.
- See page 27.

## System Objects

### Player

These are the actual humans playing the game. "Ingame" players are Nations and are considered a Game Object rather than a System Object, and are stored as part of the Turn state.

### Turn

If turn has been resolved (the turn is historical), turn additionally contains snapshots (or references to snapshots) of timers/tiles/orders/logs/etc from the moment when turn resolution is complete.

> **Note:** for the purposes scheduling timers, the phrases "end of Fall 1861" and "start of Winter 1861" are practically interchangeable. Both will have the associated events (ex. authorized loan payments) resolve during the resolution of Fall 1861. Log entries, however, will be grouped under the mentioned season (Fall 1861 or Winter 1861 in the above examples).

### Order

Record of an order submitted on a turn, or intended to be submitted on a turn if the turn has not yet resolved.

### Timer

Created as a side effect of order application or turn resolution. The timer state changes each turn.

Examples

- Income every Spring and Autumn from connected provinces
- Pay military upkeep every Spring
- Income from Prestige Achievements once per year
- Loans
- Trade agreements; publicity requirements and payment schedules
- Treaty expiration dates and milestones
- Building construction
- Unit production
- Diplomatic integration. Includes multiple stages of scheduled payments on condition that some military force is present, among others. See pages 13-14.
- Military occupation of a province. Note: Refund half cost of in-progress construction at time of invasion (p.15). Roll to capture enemy Artillery (p.22).
- Use of captured superior military technology (Artillery, Warships, Battleships) for 1 year before salvaging for half of unit cost.
- Abandoned ship waits 4 seasons to be recovered, before being removed from play. See page 26 for capturing abandoned ships.
- Forceful annexation: after occupation, nation opts to annex. Takes 4 years with special risks. Nationalist revolt checks every Summer until integration. Annexation paused while nationalist rebels occupy the province. See p.17 for details.
- Puppeting: after occupation, nation opts to puppet. Takes 4 years with special limitations.

Timers must:

- process stages/milestone events (start, progress event(s), completion)
- perform validation checks every turn (after each stage, the validation checks may change)
- be able to be paused or otherwise modified arbitrarily by the system

### Trigger

Similar to a Timer, but instead of firing on turn number changes, it fires when specific conditions of turn state are satisfied.

### Event Log

Record of events which were produced during order application and turn resolution. Events are tagged with meta-data to make them more easily categorizable and searchable.

### Notification Log

Notifications indicate required action or the actions of others involving the Nation - for example, notification that a scheduled payment is due this turn and requires authorization, or that a province has been invaded by another Nation, or that a diplomatic message has arrived. Notifications are for events which are specifically relevant to a particular Nation planning to submit orders (especially) during this turn and in future turns.

## System Events

These are logged turn state trasformations that are invoked by Timers/Triggers, applied orders, and turn resolution.

- Any event may also produce a directly associated Notification which will not be listed in the below notification section.

### Prestige Awarded

### Reactionary Revolt

### Nationalist Revolt

### Trade Complete

### Loan Complete

### Canal Complete

### Mustering Completion

### Construction Complete

### International Payment

### Land Combat

### Naval Combat

## System Notifications (Side-effects)

The logged notifications listed here are generated as side-effects of state transformation.

### Invasion

### Military Victory

### Military Defeat
