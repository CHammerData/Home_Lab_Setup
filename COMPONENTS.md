# Home Lab — Hardware Inventory & Component Tracker

Tracks all physical hardware across every node in the rack. Update the **Status** column as components are ordered, received, and installed.

**Status Key:** `Owned` | `To Order` | `Ordered` | `Received` | `Installed` | `Deferred`

---

## Summary

| Node | Expected Total | Actual Spent | Still to Go | Status |
| :--- | ---: | ---: | ---: | :--- |
| Shared / Rack | $930.00 | $0.00 | $930.00 | Not Started |
| Desktop Conversion | $1,111.00 | $351.49 | $700.00 | In Progress |
| NAS Node | $1,857.00 | $298.24 | $1,476.00 | In Progress |
| App Server | $1,711.00 | $891.61 | $768.60 | In Progress |
| **GRAND TOTAL** | **$5,609.00** | **$1,541.34** | **$3,874.60** | **In Progress** |

> **DDR5 Note:** RAM prices reflect the active DDR5 shortage (prices up ~300–400% since mid-2025, forecast to persist through Q4 2027). Monitor for price drops before purchasing if timeline allows.

---

## Shared / Rack

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | :--- | :--- |
| Rack Chassis | Rosewill RSV-L4500U 4U Server Chassis | 3 | $230.00 | | $690.00 | | To Order | One each for NAS, App Server, and Desktop. |
| Switch | TP-Link TL-SG1024DE 24-Port Easy Smart Managed | 1 | $120.00 | | $120.00 | | To Order | Managed. Supports VLANs for network segmentation. |
| Patch Panel | TRENDnet 24-Port Blank Keystone 1U + Cat6 | 1 | $120.00 | | $120.00 | | To Order | |
| Rack Enclosure | — | 1 | Owned | $0.00 | $0.00 | $0.00 | Owned | Existing rack. |
| UPS | — | — | — | — | — | — | Deferred | Revisit after initial build. |

**Shared / Rack Expected Total: $930.00**
**Shared / Rack Actual Spent: $0.00**
**Shared / Rack Still to Go: $930.00**

---

## Desktop Conversion (Gaming PC)

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | :--- | :--- |
| GPU Upgrade | ASUS ProArt GeForce RTX 4070 Super OC | 1 | $700.00 | | $700.00 | | To Order | Replaces RTX 2070 Super (moved to App Server). Card is 266mm — confirmed fits RSV-L4500U. |
| Memory | 32GB Corsair VENGEANCE RGB DDR5 | 1 | $411.00 | $351.49 | $411.00 | $351.49 | Ordered | DDR5 shortage pricing — premium kit. |
| Rack Chassis | Listed in Shared / Rack | — | — | — | — | — | — | — |

**Desktop Conversion Expected Total: $1,111.00**
**Desktop Conversion Actual Spent: $351.49**
**Desktop Conversion Still to Go: $700.00** *(GPU not yet ordered)*

---

## NAS Node

**OS:** TrueNAS SCALE
**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i3-12100 | 1 | $110.00 | $95.61 | $110.00 | $95.61 | Ordered | LGA1700 socket. |
| Motherboard | ASRock B760 PRO RS | 1 | $150.00 | $109.99 | $150.00 | $109.99 | Ordered | LGA1700, DDR5, 6x SATA ports. |
| Memory | 16GB DDR5 | 1 | $0.00 | $0.00 | $0.00 | $0.00 | Owned | Pulled from gaming desktop. |
| Power Supply | Corsair RM650e 650W | 1 | $121.00 | $92.64 | $121.00 | $92.64 | Ordered | 80+ Gold. Adequate for i3-12100 + 4x HDD. |
| Storage (HDD) | Seagate IronWolf 12TB NAS HDD | 4 | $369.00 | | $1,476.00 | | To Order | ZFS RAID-Z1 (~36TB usable). |

**NAS Node Expected Total: $1,857.00**
**NAS Node Actual Spent: $298.24**
**NAS Node Still to Go: $1,476.00** *(4x IronWolf HDDs not yet ordered)*

---

## App Server

**OS:** Ubuntu Server LTS
**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Actual Price | Expected Total | Actual Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i5-13500 | 1 | $245.00 | $297.49 | $245.00 | $297.49 | Ordered | LGA1700, 14 cores / 20 threads. |
| Motherboard | Gigabyte Z790 UD AX | 1 | $180.00 | $149.99 | $180.00 | $149.99 | Ordered | LGA1700, DDR5, PCIe 5.0. Supports GPU passthrough. |
| Memory | 32GB Corsair VENGEANCE RGB DDR5 | 1 | $411.00 | $351.49 | $411.00 | $351.49 | Ordered | DDR5 shortage pricing — premium kit. |
| Power Supply | Corsair RM750e 750W | 1 | $121.00 | $92.64 | $121.00 | $92.64 | Ordered | 80+ Gold. Adequate for i5-13500 + RTX 2070 Super. |
| Storage (NVMe) | Samsung 980 PRO 2TB | 2 | $377.00 | $384.30 | $754.00 | $768.60 | To Order | Primary OS + Docker working directory. Second drive for additional storage or backup staging. |
| GPU | Nvidia RTX 2070 Super | 1 | Owned | $0.00 | $0.00 | $0.00 | Owned | Pulled from gaming desktop. Jellyfin NVENC hardware transcoding. |

**App Server Expected Total: $1,711.00**
**App Server Actual Spent: $891.61** *(CPU, Mobo, Memory, PSU ordered)*
**App Server Still to Go: $768.60** *(2x NVMe drives not yet ordered)*

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
