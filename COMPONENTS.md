# Home Lab — Hardware Inventory & Component Tracker

Tracks all physical hardware across every node in the rack. Update the **Status** column as components are ordered, received, and installed.

**Status Key:** `Owned` | `To Order` | `Ordered` | `Received` | `Installed` | `Deferred`

**Saved / (Over):** Positive = under budget. Parentheses = over budget.

---

## Summary

| Node | Expected Total | Actual Spent | Still to Go | Saved / (Over) | Status |
| :--- | ---: | ---: | ---: | ---: | :--- |
| Shared / Rack | $1,295.00 | $1,260.99 | $0.00 | +$34.01 | Complete ✓ |
| Desktop Conversion | $1,111.00 | $1,146.24 | $0.00 | ($35.24) | Complete ✓ |
| NAS Node | $1,882.00 | $1,643.50 | $0.00 | +$238.50 | Complete ✓ |
| App Server | $1,711.00 | $1,660.21 | $0.00 | +$50.79 | Complete ✓ |
| **GRAND TOTAL** | **$5,999.00** | **$5,710.94** | **$0.00** | **+$288.06** | **Complete ✓** |

> **DDR5 Note:** RAM prices reflect the active DDR5 shortage (prices up ~300–400% since mid-2025, forecast to persist through Q4 2027). Monitor for price drops before purchasing if timeline allows.

---

## Shared / Rack

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Saved / (Over) | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | ---: | :--- | :--- |
| Rack Chassis | Rosewill RSV-L4500U 4U Server Chassis | 3 | $230.00 | $223.11 | $690.00 | $669.34 | +$20.66 | Received | One each for NAS, App Server, and Desktop. |
| Switch | TP-Link TL-SG1024DE 24-Port Easy Smart Managed | 1 | $120.00 | $122.47 | $120.00 | $122.47 | ($2.47) | Received | Managed. Supports VLANs for network segmentation. |
| Patch Panel | Monoprice 24-Port Unloaded STP Keystone 1U | 1 | $120.00 | $34.95 | $120.00 | $34.95 | — | Received | $120 was bundled estimate for full cabling setup (all rows below). |
| Keystone Couplers | Cat 6 Keystone Couplers (24-pack) | 1 | — | $15.61 | — | $15.61 | — | Received | Included in patch panel budget above. |
| Patch Cables (6FT) | Cat 6 Ethernet Cable 6FT 10-Pack | 1 | — | $23.36 | — | $23.36 | — | Received | Short runs between patch panel and switch. |
| Patch Cables (0.5FT) | Cat 6 Ethernet Cable 0.5FT 10-Pack | 1 | — | $20.26 | — | $20.26 | — | Received | Ultra-short runs / same-rack connections. |
| Rack Hardware | M6 x 16mm Cage Nuts, Screws & Washers (50-pack) | 1 | — | $10.00 | — | $10.00 | — | Received | For mounting rack equipment. |
| Rack Enclosure | — | 1 | Owned | $0.00 | $0.00 | $0.00 | — | Owned | Existing rack. |
| UPS | CyberPower CP1500PFCRM2U PFC Sinewave 1500VA/1000W 2U | 1 | $365.00 | $365.00 | $365.00 | $365.00 | $0.00 | Received | Rackmount 2U. Pure sine wave — safe for all PSU types. |

**Shared / Rack Expected Total: $1,295.00**

**Shared / Rack Actual Spent: $1,260.99**

**Shared / Rack Still to Go: $0.00** ✓

**Shared / Rack Saved / (Over): +$34.01**

---

## Desktop Conversion (Gaming PC)

**OS:** Windows 11 (already installed and licensed)

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Saved / (Over) | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | ---: | :--- | :--- |
| GPU Upgrade | ASUS ProArt GeForce RTX 4070 Super OC | 1 | $700.00 | $794.75 | $700.00 | $794.75 | ($94.75)| Received | Replaces RTX 2070 Super (moved to App Server). Card is 266mm — confirmed fits RSV-L4500U. |
| Memory | 32GB Corsair VENGEANCE RGB DDR5 | 1 | $411.00 | $351.49 | $411.00 | $351.49 | +$59.51 | Received | DDR5 shortage pricing — premium kit. |
| Rack Chassis | Listed in Shared / Rack | — | — | — | — | — | — | — | — |

**Desktop Conversion Expected Total: $1,111.00**

**Desktop Conversion Actual Spent: $1,146.24** *(all items ordered)*

**Desktop Conversion Still to Go: $0.00** ✓

**Desktop Conversion Saved / (Over): ($35.24)**

---

## NAS Node

**OS:** TrueNAS SCALE

**Boot Device:** M.2 NVMe SSD (dedicated — see table below)

> **Note:** TrueNAS SCALE 24.10+ actively warns against USB boot devices and writes continuously to the boot pool at idle. A dedicated internal SSD is required. USB thumb drives are not supported for this use case.

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Saved / (Over) | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i3-12100 | 1 | $110.00 | $95.61 | $110.00 | $95.61 | +$14.39 | Received | LGA1700 socket. |
| Motherboard | ASRock B760 PRO RS | 1 | $150.00 | $109.99 | $150.00 | $109.99 | +$40.01 | Received | LGA1700, DDR5, 6x SATA ports. |
| Memory | 16GB DDR5 | 1 | $0.00 | $0.00 | $0.00 | $0.00 | — | Owned | Pulled from gaming desktop. |
| Power Supply | Corsair RM750e 750W | 1 | $121.00 | $92.64 | $121.00 | $92.64 | +$28.36 | Received | 80+ Gold. Adequate for i3-12100 + 4x HDD. |
| Boot Drive (M.2 SSD) | Inland TN320 256GB NVMe PCIe Gen 3.0x4 M.2 | 1 | $25.00 | $64.99 | $25.00 | $64.99 | ($39.99) | Received | Dedicated TrueNAS boot drive. USB not supported as of SCALE 24.10. M.2 slot on B760 PRO RS. |
| Storage (HDD) | Seagate IronWolf 12TB NAS HDD | 3 | $369.00 | $318.74 | $1,107.00 | $956.22 | +$150.78 | Received | ZFS RAID-Z1 (~36TB usable). |
| Storage (HDD) | WD Red Pro 12TB 7200RPM SATA III 3.5" | 1 | $369.00 | $324.05 | $369.00 | $324.05 | +$44.95 | Received | 4th drive to complete RAID-Z1. Mixed models are compatible in ZFS. |

**NAS Node Expected Total: $1,882.00**

**NAS Node Actual Spent: $1,643.50**

**NAS Node Still to Go: $0.00** ✓

**NAS Node Saved / (Over): +$238.50**

---

## App Server

**OS:** Ubuntu Server LTS

**Boot Device:** Samsung 980 PRO 2TB NVMe (Disk 1 of 2)

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Saved / (Over) | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i5-13500 | 1 | $245.00 | $297.49 | $245.00 | $297.49 | ($52.49) | Received | LGA1700, 14 cores / 20 threads. |
| Motherboard | Gigabyte Z790 UD AX | 1 | $180.00 | $149.99 | $180.00 | $149.99 | +$30.01 | Received | LGA1700, DDR5, PCIe 5.0. Supports GPU passthrough. |
| Memory | 32GB Corsair VENGEANCE RGB DDR5 | 1 | $411.00 | $351.49 | $411.00 | $351.49 | +$59.51 | Received | DDR5 shortage pricing — premium kit. |
| Power Supply | Corsair RM750e 750W | 1 | $121.00 | $92.64 | $121.00 | $92.64 | +$28.36 | Received | 80+ Gold. Adequate for i5-13500 + RTX 2070 Super. |
| Storage (NVMe) | Samsung 980 PRO 2TB | 2 | $377.00 | $384.30 | $754.00 | $768.60 | ($14.60) | Received | Disk 1: Ubuntu OS boot + Docker working directory. Disk 2: Additional storage / backup staging. |
| GPU | Nvidia RTX 2070 Super | 1 | Owned | $0.00 | $0.00 | $0.00 | — | Owned | Pulled from gaming desktop. Jellyfin NVENC hardware transcoding. |

**App Server Expected Total: $1,711.00**

**App Server Actual Spent: $1,660.21** *(all items ordered or owned)*

**App Server Still to Go: $0.00** ✓

**App Server Saved / (Over): +$50.79**

---

## Open Items / Decisions Pending

| # | Item | Decision Needed |
| :--- | :--- | :--- |
| ~~1~~ | ~~Desktop rack chassis~~ | ~~Resolved~~ — GPU is 266mm, confirmed fits RSV-L4500U. |
| ~~2~~ | ~~UPS~~ | ~~Resolved~~ — CyberPower CP1500PFCRM2U acquired. |
| ~~3~~ | ~~Switch~~ | ~~Resolved~~ — upgrading to TL-SG1024DE (managed). |

---

## Rack Layout (Planned)

Layout is top-down. Pi shelf and Home Assistant sit at the top with open space above for cabling and expansion. UPS, patch panel, and switch form the middle utility block. The three 4U chassis anchor the bottom.

> **UPS weight note:** The CP1500PFCRM2U weighs ~44 lbs with batteries. Mid-rack placement is workable but ensure the rack rails can support the load at that height. Power cables will run downward to the chassis below, which keeps things tidy.

| Position | Device | U Size | Notes |
| :--- | :--- | :---: | :--- |
| Top — open | *(expansion / cabling space)* | TBD | Reserved for Pi overhead cabling and future additions. |
| Top | Raspberry Pi shelf | TBD | Minimal depth; exact U count depends on shelf model chosen. |
| Top | Home Assistant machine | TBD | Exact form factor TBD. |
| Middle | CyberPower CP1500PFCRM2U UPS | 2U | Place at top of middle block so power cables drop cleanly to chassis below. |
| Middle | Patch Panel (1U) | 1U | Directly above switch — keeps patch cable runs as short as possible. |
| Middle | TP-Link TL-SG1024DE Switch | 1U | Directly below patch panel. |
| Bottom | NAS Node — Rosewill RSV-L4500U | 4U | Heaviest node; bottom placement improves stability. |
| Bottom | App Server — Rosewill RSV-L4500U | 4U | |
| Bottom | Desktop / Gaming PC — Rosewill RSV-L4500U | 4U | Bottommost. GPU exhaust vents toward floor — ensure adequate rear clearance. |

---

## Upgrade Path

Future hardware improvements in rough priority order. None are urgent — documented here to capture context and cost benchmarks when decisions come up.

| Priority | Component | Current | Target | Est. Cost | Trigger / Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Network Switch | TP-Link TL-SG1024DE (1GbE, 24-port) | TP-Link TL-SG3210XHP-M2 (8× 2.5GbE + 2× 10GbE SFP+) | ~$160 | App Server (Z790 UD AX) and NAS (B760 PRO RS) both have 2.5GbE NICs currently capped at 1GbE. No impact on media streaming; bottleneck shows during bulk NAS transfers (~110 MB/s real-world vs ~250 MB/s possible). |
| 2 | Gaming Desktop — Motherboard | DDR4-era board (existing hardware) | Modern LGA1700 DDR5 motherboard | ~$150–200 | DDR5 RAM (32GB Corsair VENGEANCE RGB, see Desktop Conversion) already purchased and waiting. Upgrade motherboard to unlock it. |
| 3 | Gaming Desktop — RAM | DDR4 (existing) | 32GB Corsair VENGEANCE RGB DDR5 | Already purchased | Paired with motherboard upgrade above — install both at the same time. The DDR5 kit is on the shelf waiting for the board. |
| 4 | Router | Asus consumer router | pfSense / OPNsense appliance (e.g., Protectli VP2420) | ~$300–400 | Current Asus + Pi-hole setup is adequate for the lab as-is. Upgrade when VLAN segmentation (IoT isolation, guest network, lab separation) becomes a priority. The TL-SG1024DE already supports 802.1Q VLANs — the router is the only missing piece. |
