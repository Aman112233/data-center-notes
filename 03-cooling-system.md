# Data Center Cooling System

## The Core Problem

Servers generate massive amounts of heat constantly. If that heat is not removed the equipment throttles, fails, or catches fire. The entire cooling system exists to remove that heat 24/7.

## Hot Aisle / Cold Aisle Containment

- Racks are arranged so the fronts face each other (cold aisle) and the backs face each other (hot aisle)
- Cold air is delivered into the cold aisle and blown through the servers front to back
- Hot exhaust air exits into the hot aisle and is captured and removed
- Keeps cold and hot air separated — without this they mix and cooling efficiency drops badly

## CRAH Unit (Computer Room Air Handler)

- Large units mounted on the side of the hall
- Job is to cool the air inside the data center
- Does not generate its own cooling — depends on chilled water supplied by the chiller plant
- Has coils inside — cold water runs through them, a fan blows warm room air over the coils, air cools down and is pushed back into the cold aisle
- Warm water then travels back through the pipes to the chiller to be cooled again

## Chilled Water Pipework

- Large insulated pipes running throughout the building
- Two pipes per circuit — one carries cold water from chiller to CRAH, one carries warm water back from CRAH to chiller
- The insulation prevents condensation forming on the outside of the cold pipes and prevents heat gain from the room

## Chiller

- Large machine that continuously cools water down to a set temperature
- This site has air-cooled chillers — large fans on top dump heat into the outside air
- Works using a refrigerant loop with four stages:
  - **Compressor** — takes refrigerant gas and compresses it, making it hot and high pressure
  - **Condenser** — hot refrigerant passes through here, fans blow outside air over it, heat is dumped to outside air, refrigerant cools and becomes liquid
  - **Expansion valve** — liquid refrigerant passes through this, pressure drops suddenly, refrigerant becomes very cold
  - **Evaporator** — cold refrigerant runs next to the building water, absorbs heat from it, water becomes cold and gets pumped back to the CRAH units, refrigerant turns back to gas and returns to compressor
- That cycle repeats continuously — end result is cold water always leaving the chiller toward the building

## The Full Chilled Water Loop

1. Chiller cools water down
2. Cold water pumped through pipes to CRAH units inside
3. CRAH blows cold air into cold aisle
4. Air passes through servers front to back, absorbing heat
5. Hot air exits into hot aisle, gets drawn back into CRAH
6. CRAH coils absorb that heat into the water
7. Warm water travels back through pipes to chiller
8. Chiller removes the heat and cools the water again
9. Loop repeats continuously

## FAHU (Fresh Air Handling Unit)

- Brings fresh outside air into the data center
- Filters it first — removes dust, particles, and contaminants before air enters the hall
- Two reasons fresh air matters — oxygen replenishment and pressure management inside the data center
- Most importantly manages humidity

## Humidity Control

- Data center must maintain humidity within a safe range — typically 40–60% relative humidity
- Too high — moisture condenses on equipment, causes corrosion and short circuits
- Too low — static electricity builds up, can damage sensitive components
- FAHU handles this by either:
  - **Humidifying** — adds moisture to incoming air if too dry
  - **Dehumidifying** — removes moisture from incoming air if too humid
- This treated air is then introduced into the data center environment

## How Everything Connects

- Chiller — cools the water
- Pipework — carries cold water to CRAH and warm water back
- CRAH — uses cold water to cool room air, pushes it into cold aisle
- Hot/cold aisle containment — makes sure cold and hot air don't mix
- FAHU — controls air quality and humidity of air entering the data center

---

> **Key thing to remember**
>
> The chiller does not cool the air directly. It cools the water. The CRAH uses that cold water to cool the air. They are two separate systems working together as one loop.
