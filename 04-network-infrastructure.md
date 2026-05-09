# Network Infrastructure

## The Core Idea

A data center network is not a regular office network. It has to handle massive traffic volumes between thousands of servers simultaneously, tolerate zero downtime, maintain low and predictable latency, and scale without requiring a full redesign. Every design decision in DC networking exists to meet those requirements.

## Physical Cabling Standards

- **Copper Cat6/Cat6A** — used for short runs, server to EoR switch or patch panel, maximum reliable distance 100 metres, cheaper but bulkier
- **Fibre optic** — used for all backbone runs, switch to switch, MMR to white space, floor to floor. Two types:
  - SMF (Single Mode Fibre) — thin core, laser light, long distances, yellow jacket
  - MMF (Multi Mode Fibre) — wider core, LED light, shorter distances, orange or aqua jacket
- **DAC (Direct Attach Copper)** — short twinax cable with transceivers already attached on both ends, used for very short high speed connections between nearby switches, cheaper than fibre for those distances
- Power and data cables always run in separate trays — never mixed, interference and safety reasons

## Spine and Leaf Topology

- Modern DC networking uses spine and leaf — replaced the old three-tier design
- Old three-tier had core, distribution and access layers — traffic had to travel multiple hops up and down, failures in the middle caused large outages
- **Leaf switches** — bottom layer, connect directly to servers or EoR switches, one per row or rack depending on design
- **Spine switches** — top layer, high capacity, only connect to leaf switches, never to servers directly
- Every leaf connects to every spine — always
- No leaf connects to another leaf directly
- No spine connects to another spine directly
- Result — any server reaches any other server in exactly two hops, up to spine and down to destination leaf
- If one spine goes down traffic redistributes across remaining spines automatically — no downtime
- Adding capacity means adding a new leaf and connecting it to all spines — no redesign needed

## EoR Switch (End of Row)

- This site uses EoR switches in the data halls
- One EoR switch sits at the end of each row and serves all racks in that row
- Servers in each rack run copper Cat6 cables horizontally along the row via overhead cable tray to the EoR switch
- EoR then uplinks to spine switches via fibre
- EoR is the leaf layer — all spine and leaf rules apply exactly the same way
- Fewer switches needed compared to ToR design — one EoR covers an entire row
- Easier to manage — fewer devices to monitor
- Tradeoff is longer horizontal copper runs but still within the 100 metre Cat6 limit
- Each EoR has redundant uplinks to multiple spine switches — losing one spine path does not isolate the row
- **If an EoR switch fails — every server in that entire row loses connectivity**
- EoR failure is higher priority than a single rack issue because the blast radius is the entire row
- As WSO — report immediately, escalate to network team, document every affected rack in the row for the incident report

## ToR Switch (Top of Rack)

- ToR (Top of Rack) is an alternative leaf design — one switch mounted at the top of each rack
- Servers connect directly down into the ToR with very short cables
- ToR uplinks to spine via fibre
- This site uses EoR not ToR — but you will encounter ToR in other facilities
- ToR failure takes one rack offline, EoR failure takes one entire row offline

## Patch Panels

- A patch panel is a passive panel — ports on the front, permanent cable terminations on the back
- Cables from servers or equipment run to the back and terminate there permanently
- Short patch jumpers on the front connect those ports across to active equipment
- When a connection needs to change — only the front patch jumper moves, the permanent cable behind stays untouched
- Makes moves, adds and changes (MAC work) fast and clean without disturbing permanent infrastructure
- Patch panels handle physical connections only — they have no intelligence
- Logical changes like VLANs, port speed, and port security are configured on the switch by the network team, not at the patch panel

## MMR, ODF and TR Rooms

- MMR (Meet-Me Room) is where the outside world connects into the data center — external carriers and telecom providers physically terminate their fibre cables here
- This site has MMR-A and MMR-B — same redundancy logic as Feed A and Feed B on power, two separate entry points so a single fibre cut, flood, or physical damage cannot take out both paths simultaneously
- The two MMRs are on opposite sides of the building with separate cable entry conduits deliberately — one physical event should never affect both
- Inside each MMR this site has two sub-rooms — the ODF room and the TR room — both contained within the MMR itself
- ODF (Optical Distribution Frame) room is where external fibre terminates — raw fibre coming in from outside via OSP (Outside Plant) lands here first, gets organised and made available for internal patching
- OSP is the external fibre route — this is the physical point where the carrier hands off their fibre into the building
- ODF is passive — no powered equipment, just termination and patching, fibre only
- From the ODF, internal fibre jumpers patch across to the TR room
- TR (Telecommunications Room) is where active network equipment lives — switches, patch panels, anything powered that takes the fibre from the ODF and distributes it across white space to the racks
- Grounding in the MMR is critical — fibre carries no electrical current but all metal enclosures, cable trays, ODF frames, and active TR equipment must be bonded to the facility ground bus
- Proper grounding prevents static buildup, protects equipment from surge and fault current, and ensures safety for anyone working inside
- All metal infrastructure in the MMR connects to a dedicated ground bar inside the room which ties back into the main facility grounding system

## Structured Cabling

- Horizontal cabling — runs from rack or server to EoR switch or patch panel, copper Cat6/Cat6A, stays within the same floor
- Backbone cabling — runs between floors, MMR to white space, spine to leaf — always fibre
- Colour coding used to identify cable types quickly:
  - Blue — standard data
  - Yellow — SMF fibre
  - Orange or Aqua — MMF fibre
  - Red — critical links or out of band management
  - Grey — general patch cables
- Every cable in a properly run DC is labelled at both ends — source port and destination port
- **Unlabelled cables are a serious operational risk** — faults cannot be traced quickly and wrong cables get pulled

## Tracing a Fault — Physical Path from Server to MMR

- This is the responsibility of the cabling team, not WSO — but you need to know the path to guide them and update the incident log
- Start at the server — check which NIC port the cable is in, note the cable label
- Trace horizontally along the row via overhead cable tray to the EoR switch or patch panel
- From EoR uplink — fibre jumper runs overhead via fibre tray toward the MMR
- Into the MMR — fibre arrives at the ODF, terminates on a specific ODF port, check the label matches documentation
- ODF to TR — short fibre jumper patches from ODF port across to active equipment in TR room
- TR active equipment — check the spine switch port where that fibre lands, check link light, check label
- At each step — is the cable seated properly, does the label match documentation, is the link light active
- Most faults are found at termination points — ODF ports, patch panel ports, or a loose transceiver in a switch port
- WSO role — guide the cabling team to the right rack and row, pull up documentation, update the incident log at each step

## How It All Connects

- External fibre enters via OSP into ODF in MMR-A or MMR-B
- ODF patches across to active TR equipment via fibre jumper
- TR equipment connects via backbone fibre through overhead trays into white space
- In white space fibre runs to spine switches
- Spine switches connect down to EoR switches at the end of each row via fibre uplinks
- EoR switches connect via copper Cat6 horizontally to each server in the row
- Every EoR connects to every spine — two hop path between any two servers in the facility
- Redundancy at every layer — dual MMR, redundant spine uplinks per EoR, dual NIC per server connecting to separate EoR or separate ports

---

> **Key things to remember**
>
> | Concept | Key Point |
> |---|---|
> | Spine and Leaf | Flat, two hop, every leaf to every spine, no leaf to leaf, no spine to spine |
> | EoR failure | Entire row down, higher priority incident than a single rack |
> | ODF | Passive, fibre only, termination and patching, lives in MMR |
> | Patch panel | Passive, physical connections only, logical changes happen at the switch |
> | VLAN changes | Configured on the switch by network team, patch panel does not move |
> | Cable labels | Both ends always, unlabelled cables are an operational risk |
> | MMR-A and MMR-B | Redundant external entry points, opposite sides of building, separate conduits |
> | SMF | Single mode fibre, long distance, yellow jacket |
> | MMF | Multi mode fibre, shorter distance, orange or aqua jacket |
> | DAC | Direct attach copper, short high speed connections between nearby switches |
> | EoR vs ToR | EoR = one switch per row, ToR = one switch per rack |
