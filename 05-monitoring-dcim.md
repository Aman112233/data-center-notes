# Monitoring & DCIM

## The Core Idea

A data center is a 24/7 operation. Equipment does not send you a calendar invite before it fails. Without monitoring you are blind — you find out something went wrong only after the impact is already happening. Monitoring exists to detect problems before they cause outages, prove SLA compliance, analyse capacity trends, and maintain a full audit trail of every event and action taken.

## What Gets Monitored

**Environmental monitoring — physical conditions**

- Temperature — inlet and outlet of every rack, room ambient, outside air temp
- Humidity — room level, monitored against the 40–60% RH target
- Airflow — some facilities have airflow sensors to detect containment breaches
- Water/leak detection — sensors under raised floor, near pipework, near CRAHs, near any water-bearing equipment
- Smoke and fire — pre-action detection

**Power monitoring — electrical conditions**

- Utility feed status — is SCECO power live on both feeds
- Generator status — running, on standby, fuel level, runtime hours
- UPS status — battery health, load percentage, bypass status, estimated runtime
- PDU load — per rack power consumption, per phase load, current draw
- PUE — calculated continuously from total facility power vs IT load

**IT equipment monitoring — device health**

- Server status — ping, SNMP, iDRAC/iLO out of band management
- Switch status — port up/down, link speed, traffic utilisation
- Temperature inside servers — CPU temp, inlet temp reported by the server itself
- Storage and network device health

## SCECO Feed Loss — Event Sequence and Alarm Order

When SCECO Feed A goes down the monitoring alert sequence fires in this order:

- **Utility loss alarm** — SCECO Feed A failure detected immediately at ATS/MV room level, BMS throws the alert first
- **ATS transfer alarm** — ATS (Automatic Transfer Switch) switches source, monitoring logs the transfer event and timestamps it
- **Generator start alarm** — generator cranks and comes online, monitoring confirms it picked up load
- **UPS on battery alarm** — during the transition the UPS bridges the gap on battery, monitoring shows battery mode active then clears when generator stabilises
- **PDU feed alarm** — if any PDU lost feed momentarily that logs too
- **Normalisation confirmations** — once generator is stable and load confirmed transferred, alarms clear one by one and monitoring shows green

WSO role during this event:

- Stay with GS team and collect timestamps from monitoring for each event
- Report to emergency group with a clear timeline
- Raise the EAP (Emergency Action Plan) form documenting the full incident from first alarm to resolution
- Provide continuous updates until the situation is normalised

## BMS — Building Management System

- BMS is the central monitoring platform for all mechanical and electrical infrastructure
- Watches power, cooling, temperature, humidity, water detection across the whole facility
- Grey space team operates BMS at all times from the BMS room
- WSO interacts with BMS as read-only during routine rounds — twice per shift to monitor and report temperature and humidity stability
- BMS raises alarms when thresholds are breached or equipment changes state
- Everything logged with timestamps — who acknowledged, when, what action was taken
- If WSO notices a BMS alarm during rounds — inform GS team directly, confirm whether they have acknowledged it, escalate and log if they have not
- BMS is not the same as DCIM — BMS focuses on building infrastructure, DCIM focuses on IT infrastructure and capacity. In many facilities they are separate systems that feed into each other

## DCIM — Data Center Infrastructure Management

DCIM is a software platform that gives a unified view of the entire data center — power, cooling, space, and IT assets all in one place. Sits above BMS and individual device monitoring, aggregates everything into one dashboard.

**What DCIM does:**

- **Asset management** — every rack, server, switch, PDU tracked with location, specs, and status. Know exactly what is in every U slot in every rack
- **Power monitoring and capacity** — live power draw per rack, per row, per hall. Shows how much capacity is used and how much headroom remains
- **Thermal monitoring** — temperature mapped across the floor, hotspots identified visually on a floor map
- **Space management** — tracks U space available per rack, helps plan where new equipment goes
- **Change management** — logs every move, add, change in the facility
- **Reporting and SLA** — generates uptime reports, PUE trends, capacity forecasts for management

**This site uses DCE** — the internal platform that monitors and displays all alarms across the data halls. Functions as the DCIM layer for WSO operations.

**DCIM platforms used across the industry:**

- Nlyte
- Sunbird DCIM
- Device42
- Vertiv TRELLIS
- Schneider EcoStruxure IT

## Sensors and How Data Gets to DCIM

**Temperature and humidity sensors**

- Placed at rack inlet — measures cold air temperature entering the rack
- Placed at rack outlet — measures hot air temperature leaving the rack
- Room ambient sensors — overall hall temperature
- Delta between inlet and outlet tells you how hard equipment is working — bigger delta means more heat being generated

**Power sensors**

- Built into intelligent PDUs — measure current draw per outlet, per phase
- UPS sends power data via network — load percentage, battery level, input/output voltage
- Meters at main switchgear feed facility-level power data for PUE calculation

**Protocols used to collect data:**

- **SNMP (Simple Network Management Protocol)** — most common, used by PDUs, UPS, and switches to send status and alerts to monitoring systems
- **Modbus** — used by power meters, chillers, and generators to communicate with BMS
- **IPMI / iDRAC / iLO** — out of band management interfaces built into servers, report CPU temp, fan speed, and power draw directly
- **REST API** — modern DCIM platforms pull data from devices via API calls

## PDU Going Dark in DCIM

If an intelligent PDU stops sending data to DCE — never assume it is just a comms glitch. A PDU going dark could mean it lost power entirely, which puts the rack it feeds at risk.

**Possible causes:**

- Network connectivity loss — PDU lost its IP connection to DCIM, check management port link
- PDU firmware hang — management module froze, a reset clears it
- Breaker tripped — PDU lost power entirely, GS checks breaker at TOB or busway tap-off
- SNMP misconfiguration — credentials or IP changed, PDU online but DCIM cannot authenticate
- PDU hardware fault — management card failed, PDU still distributes power but cannot report

**WSO response:**

- Acknowledge alarm in DCE immediately
- Log and report to lead, get instructions
- Inform the customer or team owning that rack
- GS team checks physical side — breakers, power, connectivity
- Do not close the alarm until data is confirmed restored and stable

## Alarm Tiers

Not all alarms are equal. DCIM and BMS use tiered alarm levels so you know how urgently to respond.

| Tier | Meaning | Example |
|---|---|---|
| Informational | State change, no action needed, logged for audit | Server powered on |
| Warning | Approaching threshold, investigate and monitor | Rack inlet temp at 26°C |
| Critical | Threshold breached, immediate action required | Rack inlet at 35°C, UPS on battery |
| Emergency | Facility level event, all hands | Fire detection, total power loss, chiller failure |

## Common Thresholds

| Parameter | Warning | Critical |
|---|---|---|
| Rack inlet temperature | 25–27°C | 30°C+ |
| Humidity | Outside 40–60% RH | At extremes |
| UPS battery | 50% charge | 20% charge |
| PDU load | 70% capacity | 80%+ |
| PUE | No hard alarm | Sudden spike triggers investigation |

Normal rack inlet target is around 22°C. PDU load should never hit 100% — always maintain headroom.

## Responding to a Temperature Warning Alarm

If a warning alarm fires on a rack inlet temperature sensor at 27°C:

- **Acknowledge the alarm in DCE immediately** — timestamps your response, shows it is being actively handled
- **Check DCE for surrounding racks** — is it isolated to one rack or is the whole row or hall trending up
- **Physical round to that rack** — visually confirm, check containment doors are closed, check for blanking panel gaps, check perforated tiles in front of the rack
- **Report to GS team** — give them the rack ID, current temp reading, trend over last hour, and whether surrounding racks are affected
- **GS investigates cooling side** — CRAH output, chilled water supply temp, whether that zone is getting adequate cold air
- **Log everything** — alarm time, acknowledgement time, who contacted, what found, action taken, resolution time
- **Monitor until resolved** — do not close the alarm until temp returns to normal and is stable

**Pattern recognition:**

- Single rack spiking = likely local issue, check blanking panels and perforated tiles first
- Entire row trending up = CRAH or containment problem, escalate to GS immediately

## Proactive vs Reactive Operations

- **Reactive** — wait for a critical alarm to fire, then respond. Problem has already escalated by the time you act
- **Proactive** — catch trends early during rounds and in DCE, investigate and report before any threshold is breached
- A temperature creeping up over 4 hours is catchable. A sudden thermal shutdown is not
- Proactive monitoring is what separates good WSO operations from average ones

## How It All Connects

- Sensors throughout the facility collect temperature, humidity, power, and equipment status continuously
- BMS aggregates mechanical and electrical data — GS team monitors and operates this
- DCIM/DCE aggregates IT and infrastructure data — WSO primary tool for operations and reporting
- Alarms fire when thresholds are breached or equipment changes state
- WSO acknowledges, logs, investigates physically, reports to relevant teams, and tracks to resolution
- Every event is timestamped and documented — feeds into EAP forms, SLA reports, and capacity planning

---

> **Key things to remember**
>
> | Concept | Key Point |
> |---|---|
> | BMS | Monitors building infrastructure — power, cooling, environment. GS operates it, WSO reads during rounds |
> | DCIM / DCE | Unified platform — assets, power, thermal, space, reporting. WSO primary tool |
> | Proactive monitoring | Act on trends before thresholds breach, do not wait for critical alarms |
> | Alarm tiers | Informational, Warning, Critical, Emergency — response urgency scales accordingly |
> | PDU dark in DCIM | Never assume comms glitch — could mean power loss, investigate immediately |
> | SNMP | Protocol PDUs and UPS use to report data to DCIM |
> | Modbus | Protocol chillers, generators, power meters use to report to BMS |
> | iDRAC / iLO | Out of band server management — reports CPU temp, power draw, fan speed |
> | Rack inlet temp | Warning 25–27°C, Critical 30°C+, normal target around 22°C |
> | PDU load | Never exceed 80%, always maintain headroom |
> | EAP form | Emergency Action Plan — raised after any significant incident with full timeline |
