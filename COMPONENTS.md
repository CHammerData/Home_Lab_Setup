# Home Lab — Hardware Inventory & Component Tracker

Tracks all physical hardware across every node in the rack. Update the **Status** column as components are ordered, received, and installed.

**Status Key:** `Owned` | `To Order` | `Ordered` | `Received` | `Installed`

---

## Summary

| Node | Est. Total | Status |
| :--- | ---: | :--- |
| Shared / Rack | $910.00 | — |
| Desktop Conversion | $700.00 | — |
| NAS Node | $2,056.00 | — |
| App Server | $1,735.00 | — |
| **GRAND TOTAL** | **$5,401.00** | — |

> **Note:** RAM prices reflect the active DDR5 shortage (prices up ~300–400% since mid-2025, forecast to persist through Q4 2027). Monitor for price drops before purchasing if timeline allows.

---

## Shared / Rack

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| Rack Chassis | Rosewill RSV-L4500U 4U Server Chassis | 2 | $230.00 | $460.00 | To Order | NAS + App Server only. See Desktop section for gaming PC chassis. |
| Switch | TP-Link 24-Port Gigabit (TL-SG1024S) | 1 | $100.00 | $100.00 | To Order | Unmanaged. Consider upgrading to TL-SG1024DE (easy smart managed) for VLAN support. |
| Patch Panel | TRENDnet 24-Port Blank Keystone 1U + Cat6 | 1 | $120.00 | $120.00 | To Order | |
| Rack Enclosure | — | 1 | Owned | $0.00 | Owned | Existing rack — verify U space before ordering chassis. |
| UPS | — | — | — | — | To Order | Strongly recommended for NAS. Consider APC BR1500G or equivalent. |

---

## Desktop Conversion (Gaming PC)

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| GPU Upgrade | ASUS ProArt GeForce RTX 4070 Super OC | 1 | $700.00 | $700.00 | To Order | Replaces RTX 2070 Super (moved to App Server). |
| Rack Chassis | TBD | 1 | TBD | TBD | To Order | **Decision pending.** RSV-L4500U has only ~330mm GPU clearance — ASUS ProArt 4070 Super OC is ~300mm, verify fitment before ordering. Alternatives: iStarUSA D-400L-6 (~330mm clearance) or a 2U rack shelf to keep existing tower case. |

---

## NAS Node

**OS:** TrueNAS SCALE
**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i3-12100 | 1 | $110.00 | $110.00 | To Order | LGA1700 socket. |
| Motherboard | ASRock B760 PRO RS | 1 | $150.00 | $150.00 | To Order | LGA1700, DDR5, 6x SATA ports. Verify SATA count supports 4 HDDs + any additional drives. |
| Memory | 16GB Crucial DDR5 | 1 | $199.00 | $199.00 | To Order | DDR5 shortage pricing. |
| Power Supply | Corsair RM650e 650W | 1 | $121.00 | $121.00 | To Order | 80+ Gold. Adequate for i3-12100 + 4x HDD. |
| Storage (HDD) | Seagate IronWolf 12TB NAS HDD | 4 | $369.00 | $1,476.00 | To Order | ZFS pool. Running RAID-Z1 (36TB usable). Note: RAID-Z2 recommended for drives this size in production, but acceptable for home lab. |

**NAS Node Total: $2,056.00**

---

## App Server

**OS:** Ubuntu Server LTS
**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i5-13500 | 1 | $245.00 | $245.00 | To Order | LGA1700, 14 cores / 20 threads. |
| Motherboard | Gigabyte Z790 UD AX | 1 | $180.00 | $180.00 | To Order | LGA1700, DDR5, PCIe 5.0. Supports GPU passthrough. |
| Memory | 32GB Corsair VENGEANCE RGB DDR5 | 1 | $411.00 | $411.00 | To Order | DDR5 shortage pricing — premium kit. |
| Power Supply | Corsair RM750e 750W | 1 | $145.00 | $145.00 | To Order | 80+ Gold. Adequate for i5-13500 + RTX 2070 Super. |
| Storage (NVMe) | Samsung 980 PRO 2TB | 2 | $377.00 | $754.00 | To Order | Primary OS + Docker working directory. Second drive for additional storage or backup staging. |
| GPU | Nvidia RTX 2070 Super | 1 | Owned | $0.00 | Owned | Pulled from gaming desktop. Used for Jellyfin NVENC hardware transcoding. |

**App Server Total: $1,735.00**

---

## Open Items / Decisions Pending

| # | Item | Decision Needed |
| :--- | :--- | :--- |
| 1 | Desktop rack chassis | Confirm GPU clearance for ASUS ProArt 4070 Super OC (~300mm) in RSV-L4500U, or select iStarUSA D-400L-6 / rack shelf alternative |
| 2 | UPS | Select model and add to order |
| 3 | Switch | Confirm unmanaged (TL-SG1024S) vs. managed (TL-SG1024DE) based on whether VLANs are needed |

---

## Rack Layout (Planned)

| U Position | Device |
| :--- | :--- |
| TBD | Patch Panel (1U) |
| TBD | TP-Link Switch (1U) |
| TBD | NAS Node — Rosewill RSV-L4500U (4U) |
| TBD | App Server — Rosewill RSV-L4500U (4U) |
| TBD | Desktop / Gaming PC (4U or shelf) |
| TBD | Designated Home Assistant Machine |
| TBD | Raspberry Pi shelf |
