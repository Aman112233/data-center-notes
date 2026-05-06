# Physical Infrastructure

## The Core Idea

A data center is not a regular building. Every aspect of it — the floor, the racks, the cables, the layout — is engineered specifically to support heavy IT equipment, manage airflow, and keep operations running without interruption.

## White Space vs Grey Space

- **White space** is the server hall — IT equipment lives here, WSO operates here
- **Grey space** is everything behind the scenes — UPS rooms, generator yards, MV rooms, chiller plant, electrical switchgear
- They are kept physically separate for safety and access control — a contractor working on electrical equipment does not need to be near IT gear
- Grey space team monitors the BMS (Building Management System) at all times — this covers power, cooling, temperature, and humidity alarms across the whole facility
- If BMS throws a critical alarm in grey space, it directly affects white space operations

## Raised Floor

- Most data centers have a raised floor sitting 600–900mm above the structural floor
- The gap underneath is called the plenum — it is pressurized with cold air from the CRAHs (Computer Room Air Handlers)
- Perforated tiles are placed in cold aisles only — cold air rises up through them into the front of racks
- Solid tiles everywhere else — prevents cold air from escaping randomly
- The plenum also routes power and data cables underneath the floor
- If perforated tiles are missing in front of a rack, no cold air reaches the equipment — servers pull in warm air, temperatures rise, equipment throttles or shuts down

## Cable Management

- Two types of cables in a data center — power cables and data/network cables
- They always run in separate trays — never mixed (interference and safety reasons)
- Cable trays — open metal trays mounted overhead or under floor that cables sit in
- Cable ladders — like trays but with rungs, better airflow around the cables
- Velcro ties used inside racks to bundle cables — never permanent zip ties, things need to move
- Patch panels — consolidation point where structured cabling terminates before connecting to switches
- This site runs both power and data overhead — easier to trace, easier to manage, consistent with overhead busway for power distribution
- Messy cabling blocks airflow inside racks, slows troubleshooting, and creates risk of pulling the wrong cable

## Rack Systems

- Standard rack is 42U — U stands for rack unit, one U = 44.45mm
- Everything mounted inside is measured in Us — 1U server, 2U UPS, etc.
- Front of rack = cold air intake, faces cold aisle
- Rear of rack = hot air exhaust, power connections, cable exits, faces hot aisle
- Mounting rails inside are adjustable — servers slide in and screw to them
- Blanking panels fill empty U slots — **critical, not cosmetic**
- Without blanking panels, hot air exhausted from servers higher up recirculates back through empty gaps and re-enters the intake of servers below — equipment pulls in already hot air
- Blanking panels block that internal bypass path completely
- Two PDUs (Power Distribution Units) per rack — Feed A and Feed B — each server PSU connects to one each for full redundancy

## Rack Types

- Open frame — no side panels, easy access, less physical security
- Enclosed cabinet — lockable doors, better for security and airflow control
- This site uses hot aisle containment which works best with enclosed or semi-enclosed racks

## Containment Structures

- Hot aisle containment — this site uses this — encloses the hot aisle with doors and roof panels, captures exhaust air and directs it straight back to CRAH return
- Cold aisle containment — alternative approach — encloses the cold aisle instead to keep cold air from escaping before reaching rack intakes
- Both approaches prevent hot and cold air mixing in white space
- With hot aisle containment, the rest of white space stays cool because no hot exhaust escapes into it
- If containment doors are left open — hot air escapes into white space, mixes with cold air, servers pull in warmer air, CRAHs work harder, efficiency drops
- **Containment doors should always be kept closed** — most have auto-close hinges for this reason
- Cage systems — metal mesh enclosures used in colocation facilities to separate individual customers, each customer gets their own locked cage

## PUE — Power Usage Effectiveness

- The key metric for measuring how efficient a data center is
- Formula: `PUE = Total Facility Power / IT Equipment Power`
- PUE 1.0 = perfect efficiency, impossible in reality
- PUE 1.3 = for every 1W servers consume, facility draws 1.3W from the grid — the extra 0.3W goes to cooling, lighting, UPS losses
- PUE 1.5 = industry average
- PUE 2.0+ = inefficient, older facility
- Hyperscale data centers like Google and Microsoft achieve 1.1–1.2
- Everything affects PUE — missing blanking panels, open containment doors, misplaced perforated tiles, chiller faults, high outdoor temperature all push PUE up
- **Lower is better** — a sudden PUE spike is a flag, investigate cooling first as that is where most overhead comes from

## How It All Connects

- Building is divided into white space (IT) and grey space (mechanical/electrical)
- Raised floor distributes cold air under the floor up through perforated tiles into rack intakes
- Racks are arranged in hot/cold aisle layout, enclosed cabinets with blanking panels keeping airflow controlled inside each rack
- Hot aisle containment captures exhaust and sends it back to CRAHs without mixing into white space
- Cables run overhead in separate trays — power and data never share the same tray
- PUE measures how efficiently all of this is working together

## MMR, ODF and TR Rooms

The MMR (Meet-Me Room) is the connectivity entry point of the facility — where the outside world physically connects into the data center.

- MMR is where external carriers and telecom providers terminate their fibre cables
- This site has MMR-A and MMR-B — same redundancy logic as Feed A and Feed B on power, two separate entry points so a single fibre cut, flood, or physical damage cannot take out both paths simultaneously
- The two MMRs are on opposite sides of the building with separate cable entry conduits deliberately — one physical event should never affect both
- Inside each MMR this site has two sub-rooms — the ODF room and the TR room — both contained within the MMR itself rather than being separate standalone rooms
- ODF (Optical Distribution Frame) room is where external fibre terminates — raw fibre coming in from outside via OSP (Outside Plant) lands here first, gets organised and made available for internal patching
- OSP is the external fibre route — this is the physical point where the carrier hands off their fibre into the building
- From the ODF, internal fibre jumpers patch across to the TR room
- ODF is passive — no powered equipment, just termination and patching
- TR (Telecommunications Room) is where active network equipment lives — switches, patch panels, anything powered that takes the fibre from the ODF and distributes it across white space to the racks
- Grounding in the MMR is critical — fibre itself carries no electrical current but all metal enclosures, cable trays, ODF frames, and active TR equipment must be bonded to the facility ground bus
- Proper grounding prevents static buildup, protects equipment from surge and fault current, and ensures safety for anyone working inside
- All metal infrastructure in the MMR connects to a dedicated ground bar inside the room which ties back into the main facility grounding system

---

> **Key things to remember**
>
> - Perforated tiles — cold aisle only, supply cold air upward into rack intakes
> - Blanking panels — block hot air recirculating inside the rack, not cosmetic
> - Containment doors — **always closed**, open doors destroy cooling efficiency immediately
> - PUE — **lower is better**, 1.3 is good, sudden increase means something changed
