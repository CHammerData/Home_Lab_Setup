# Home Lab — Hardware Inventory & Component Tracker

Tracks all physical hardware across every node in the rack. Update the **Status** column as components are ordered, received, and installed.

**Status Key:** `Owned` | `To Order` | `Ordered` | `Received` | `Installed` | `Deferred`

**Saved / (Over):** Positive = under budget. Parentheses = over budget.

---

## Summary

| Node | Expected Total | Actual Spent | Still to Go | Saved / (Over) | Status |
| :--- | ---: | ---: | ---: | ---: | :--- |
| Shared / Rack | $930.00 | $791.81 | $120.00 | +$18.19 | In Progress |
| Desktop Conversion | $1,111.00 | $1,146.24 | $0.00 | ($35.24) | Complete ✓ |
| NAS Node | $1,857.00 | $298.24 | $1,476.00 | +$82.76 | In Progress |
| App Server | $1,711.00 | $1,660.21 | $0.00 | +$50.79 | Complete ✓ |
| **GRAND TOTAL** | **$5,609.00** | **$3,896.50** | **$1,596.00** | **+$116.50** | **In Progress** |

> **DDR5 Note:** RAM prices reflect the active DDR5 shortage (prices up ~300–400% since mid-2025, forecast to persist through Q4 2027). Monitor for price drops before purchasing if timeline allows.

---

## Shared / Rack

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Saved / (Over) | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | ---: | :--- | :--- |
| Rack Chassis | Rosewill RSV-L4500U 4U Server Chassis | 3 | $230.00 | $223.11 | $690.00 | $669.34 | +$20.66 | Ordered | One each for NAS, App Server, and Desktop. |
| Switch | TP-Link TL-SG1024DE 24-Port Easy Smart Managed | 1 | $120.00 | $122.47 | $120.00 | $122.47 | ($2.47) | Ordered | Managed. Supports VLANs for network segmentation. |
| Patch Panel | TRENDnet 24-Port Blank Keystone 1U + Cat6 | 1 | $120.00 | | $120.00 | | | To Order | |
| Rack Enclosure | — | 1 | Owned | $0.00 | $0.00 | $0.00 | — | Owned | Existing rack. |
| UPS | — | — | — | — | — | — | — | Deferred | Revisit after initial build. |

**Shared / Rack Expected Total: $930.00**

**Shared / Rack Actual Spent: $791.81**

**Shared / Rack Still to Go: $120.00** *(Patch Panel not yet ordered)*

**Shared / Rack Saved / (Over): +$18.19**

---

## Desktop Conversion (Gaming PC)

**OS:** Windows 11 (already installed and licensed)

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Saved / (Over) | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | ---: | :--- | :--- |
| GPU Upgrade | ASUS ProArt GeForce RTX 4070 Super OC | 1 | $700.00 | $794.75 | $700.00 | $794.75 | ($94.75)| Ordered | Replaces RTX 2070 Super (moved to App Server). Card is 266mm — confirmed fits RSV-L4500U. |
| Memory | 32GB Corsair VENGEANCE RGB DDR5 | 1 | $411.00 | $351.49 | $411.00 | $351.49 | +$59.51 | Received | DDR5 shortage pricing — premium kit. |
| Rack Chassis | Listed in Shared / Rack | — | — | — | — | — | — | — | — |

**Desktop Conversion Expected Total: $1,111.00**

**Desktop Conversion Actual Spent: $1,146.24** *(all items ordered)*

**Desktop Conversion Still to Go: $0.00** ✓

**Desktop Conversion Saved / (Over): ($35.24)**

---

## NAS Node

**OS:** TrueNAS SCALE

**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Saved / (Over) | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i3-12100 | 1 | $110.00 | $95.61 | $110.00 | $95.61 | +$14.39 | Ordered | LGA1700 socket. |
| Motherboard | ASRock B760 PRO RS | 1 | $150.00 | $109.99 | $150.00 | $109.99 | +$40.01 | Received | LGA1700, DDR5, 6x SATA ports. |
| Memory | 16GB DDR5 | 1 | $0.00 | $0.00 | $0.00 | $0.00 | — | Owned | Pulled from gaming desktop. |
| Power Supply | Corsair RM750e 750W | 1 | $121.00 | $92.64 | $121.00 | $92.64 | +$28.36 | Ordered | 80+ Gold. Adequate for i3-12100 + 4x HDD. |
| Storage (HDD) | Seagate IronWolf 12TB NAS HDD | 4 | $369.00 | | $1,476.00 | | | To Order | ZFS RAID-Z1 (~36TB usable). |

**NAS Node Expected Total: $1,857.00**

**NAS Node Actual Spent: $298.24**

**NAS Node Still to Go: $1,476.00** *(4x IronWolf HDDs not yet ordered)*

**NAS Node Saved / (Over): +$82.76**

---

## App Server

**OS:** Ubuntu Server LTS

**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Saved / (Over) | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i5-13500 | 1 | $245.00 | $297.49 | $245.00 | $297.49 | ($52.49) | Ordered | LGA1700, 14 cores / 20 threads. |
| Motherboard | Gigabyte Z790 UD AX | 1 | $180.00 | $149.99 | $180.00 | $149.99 | +$30.01 | Received | LGA1700, DDR5, PCIe 5.0. Supports GPU passthrough. |
| Memory | 32GB Corsair VENGEANCE RGB DDR5 | 1 | $411.00 | $351.49 | $411.00 | $351.49 | +$59.51 | Received | DDR5 shortage pricing — premium kit. |
| Power Supply | Corsair RM750e 750W | 1 | $121.00 | $92.64 | $121.00 | $92.64 | +$28.36 | Received | 80+ Gold. Adequate for i5-13500 + RTX 2070 Super. |
| Storage (NVMe) | Samsung 980 PRO 2TB | 2 | $377.00 | $384.30 | $754.00 | $768.60 | ($14.60) | Received | Primary OS + Docker working directory. Second drive for additional storage or backup staging. |
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
| ~~2~~ | ~~UPS~~ | ~~Resolved~~ — deferred. Revisit after initial build. |
| ~~3~~ | ~~Switch~~ | ~~Resolved~~ — upgrading to TL-SG1024DE (managed). |

---

## Rack Layout (Planned)

| U Position | Device |
| :--- | :--- |
| TBD | Patch Panel (1U) |
| TBD | TP-Link Switch (1U) |
| TBD | NAS Node — Rosewill RSV-L4500U (4U) |
| TBD | App Server — Rosewill RSV-L4500U (4U) |
| TBD | Desktop / Gaming PC — Rosewill RSV-L4500U (4U) |
| TBD | Designated Home Assistant Machine |
| TBD | Raspberry Pi shelf |
