# Home Lab — Hardware Inventory & Component Tracker

Tracks all physical hardware across every node in the rack. Update the **Status** column as components are ordered, received, and installed.

**Status Key:** `Owned` | `To Order` | `Ordered` | `Received` | `Installed` | `Deferred`

---

## Summary

| Node | Est. Total | Status |
| :--- | ---: | :--- |
| Shared / Rack | $736.30 | — |
| Desktop Conversion | $765.71 | — |
| NAS Node | $1,833.48 | — |
| App Server | $1,257.88 | — |
| **GRAND TOTAL** | **$4,593.37** | — |

> **DDR5 Note:** RAM prices reflect the active DDR5 shortage (prices up ~300–400% since mid-2025, forecast to persist through Q4 2027). Monitor for price drops before purchasing if timeline allows.

---

## Shared / Rack

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| Rack Chassis | Rosewill RSV-L4500U 4U Server Chassis | 3 | [$189.99](https://www.newegg.com/rosewill-rsv-l4500u-black/p/N82E16811147280) | $569.97 | To Order | One each for NAS, App Server, and Desktop. |
| Switch | TP-Link TL-SG1024DE 24-Port Easy Smart Managed | 1 | [$139.99](https://www.newegg.com/tp-link-tl-sg1024de-24-port-gigabit-easysmart-switch/p/N82E16833704172) | $139.99 | To Order | Managed. Supports VLANs for network segmentation. |
| Patch Panel | TRENDnet 24-Port Blank Keystone 1U + Cat6 | 1 | [$26.34](https://www.newegg.com/trendnet-tc-kp24-black/p/N82E16816103138) | $26.34 | To Order | Price for blank panel. Jacks are separate. |
| Rack Enclosure | — | 1 | Owned | $0.00 | Owned | Existing rack. |
| UPS | — | — | — | — | Deferred | Revisit after initial build. |

---

## Desktop Conversion (Gaming PC)

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| GPU Upgrade | ASUS ProArt GeForce RTX 4070 Super OC | 1 | [$765.71](https://www.newegg.com/asus-geforce-rtx-4070-super-proart-rtx4070s-o12g/p/N82E16814126692) | $765.71 | To Order | Replaces RTX 2070 Super (moved to App Server). Card is 266mm — confirmed fits RSV-L4500U. |
| Rack Chassis | Listed in Shared / Rack | — | — | — | — | — |

---

## NAS Node

**OS:** TrueNAS SCALE
**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i3-12100 | 1 | [$122.49](https://www.newegg.com/intel-core-i3-12100-core-i3-12th-gen/p/N82E16819118389) | $122.49 | To Order | LGA1700 socket. |
| Motherboard | ASRock B760 PRO RS | 1 | [$217.00](https://www.amazon.com/ASRock-B760-Pro-RS-Motherboard/dp/B0BP93T1G3) | $217.00 | To Order | LGA1700, DDR5, 6x SATA ports. |
| Memory | 16GB Crucial DDR5 | 1 | [$218.00](https://www.amazon.com/Crucial-4800MHz-Desktop-Memory-CT16G48C40U5/dp/B09G2212SX) | $218.00 | To Order | DDR5 shortage pricing. |
| Power Supply | Corsair RM650e 650W | 1 | [$79.99](https://www.amazon.com/Corsair-RM650e-Fully-Modular-Power/dp/B0BY2P2G33) | $79.99 | To Order | 80+ Gold. Adequate for i3-12100 + 4x HDD. |
| Storage (HDD) | Seagate IronWolf 12TB NAS HDD | 4 | [$299.00](https://www.microcenter.com/product/642171/seagate-ironwolf-12tb-7200-rpm-sata-iii-6gb-s-35-internal-nas-cmr-hard-drive) | $1,196.00 | To Order | ZFS RAID-Z1 (~36TB usable). |

**NAS Node Total: $1,833.48**

---

## App Server

**OS:** Ubuntu Server LTS
**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| CPU | Intel Core i5-13500 | 1 | [$199.99](https://www.amazon.com/Intel-i5-13500-Desktop-Processor-P-cores/dp/B0BQ65CJ5L) | $199.99 | To Order | LGA1700, 14 cores / 20 threads. |
| Motherboard | Gigabyte Z790 UD AX | 1 | [$217.93](https://www.newegg.com/p/N82E16813145417) | $217.93 | To Order | LGA1700, DDR5, PCIe 5.0. Supports GPU passthrough. |
| Memory | 32GB Corsair VENGEANCE RGB DDR5 | 1 | [$249.99](https://www.microcenter.com/product/661846/corsair-vengeance-rgb-32gb-(2-x-16gb)-ddr5-6000-pc5-48000-cl36-dual-channel-desktop-memory-kit-cmh32gx5m2m6000z36-black) | $249.99 | To Order | DDR5 shortage pricing — premium kit. |
| Power Supply | Corsair RM750e 750W | 1 | [$89.99](https://www.newegg.com/corsair-rm750e-cp-9020262-na-750-w/p/N82E16817139304) | $89.99 | To Order | 80+ Gold. Adequate for i5-13500 + RTX 2070 Super. |
| Storage (NVMe) | Samsung 980 PRO 2TB | 2 | [$249.99](https://www.walmart.com/ip/SAMSUNG-980-PRO-Series-2TB-PCIe-Gen4-NVMe-Internal-Solid-State-Drive-SSD-for-Laptops-MZ-V8P2T0B-AM/234789547) | $499.98 | To Order | Primary OS + Docker working directory. Second drive for additional storage or backup staging. |
| GPU | Nvidia RTX 2070 Super | 1 | Owned | $0.00 | Owned | Pulled from gaming desktop. Jellyfin NVENC hardware transcoding. |

**App Server Total: $1,257.88**

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
