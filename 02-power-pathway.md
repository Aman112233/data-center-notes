# Data Center Power Pathway

## 1. SCECO (Utility Supply)

- Transmits high voltage (HV) over long distances via pylons
- Steps it down to MV (Medium Voltage) at their own substation off-site
- The facility only ever receives MV at the gate

## 2. MV Intake → MV Room → MV Switchgear

- MV arrives at the facility
- MV room receives it, switchgear protects and distributes it
- Voltage stays MV here — nothing is converted

## 3. Transformer (MV → LV)

- The only point where voltage actually changes
- Converts MV to LV (Low Voltage) for safe internal use

## 4. LV Switchgear

- Receives LV, protects and distributes it onward
- Voltage stays LV — again, no conversion

## 5. Generator + ATS

- Generator: diesel backup, takes ~30–40 seconds to start
- ATS (Automatic Transfer Switch): detects grid failure, transfers load to generator, transfers back when grid returns
- Note: confirm with your team whether the generator connects on the MV or LV side

## 6. UPS + Batteries

- Kicks in milliseconds after grid fails — no interruption to IT load
- Bridges the ~30–40 second gap while the generator is starting
- Once generator is stable, UPS (Uninterruptible Power Supply) stops discharging and recharges

## 7. STS (Static Transfer Switch)

- Switches between Feed A and Feed B in milliseconds
- Not for outages — purely for source redundancy between two live feeds

## 8. Distribution Panel

- Receives power from UPS/STS
- Distributes it out to the busway circuits via breakers

## 9. Overhead Busway

- Metal conductor duct running above the racks
- Carries power along the row — TOBs tap off from it

## 10. TOB (Tap-Off Box)

- Clips onto busway, has its own breaker
- Taps power down to feed one PDU below it

## 11. PDU (×2 per rack)

- Mounted inside the rack
- Two PDUs (Power Distribution Units) per rack — one on Feed A, one on Feed B

## 12. PSU → IT Load

- Built into each server/device
- Dual PSUs (Power Supply Units) — one plugged to PDU-A, one to PDU-B
- Converts AC to DC — this is where power becomes compute

---

> **Key rule to remember**
>
> - **Transformers** = change voltage
> - **Switchgear** = protect and distribute (voltage stays the same)
