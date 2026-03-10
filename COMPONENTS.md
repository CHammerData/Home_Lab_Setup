# Home Lab — Hardware Inventory & Component Tracker

Tracks all physical hardware across every node in the rack. Update the **Status** column as components are ordered, received, and installed.

**Status Key:** `Owned` | `To Order` | `Ordered` | `Received` | `Installed` | `Deferred`

> **Prices last verified:** March 2026. Prioritized Newegg and Micro Center. Amazon used as fallback only.

---

## Summary

| Node | Original Budget | Current Price | Savings |
| :--- | ---: | ---: | ---: |
| Shared / Rack | $930.00 | $831.57 | $98.43 |
| Desktop Conversion | $930.00 | $456.09 | $473.91 |
| NAS Node | $2,056.00 | $1,694.11 | $361.89 |
| App Server | $1,735.00 | $1,200.73 | $534.27 |
| **GRAND TOTAL** | **$5,651.00** | **$4,182.50** | **$1,468.50** |

> **DDR5 Note:** RAM prices still elevated due to ongoing shortage (up ~300–400% since mid-2025). Micro Center is showing lower prices than expected on some kits — verify in-store vs. online availability before ordering.

---

## Shared / Rack

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| Rack Chassis | [Rosewill RSV-L4500U 4U Server Chassis](https://www.newegg.com/rosewill-rsv-l4500u-black/p/N82E16811147328) | 3 | $229.99 | $689.97 | To Order | One each for NAS, App Server, and Desktop. |
| Switch | [TP-Link TL-SG1024DE 24-Port Easy Smart Managed](https://www.newegg.com/tp-link-tl-sg1024de-24-x-rj45/p/N82E16833704184) | 1 | $115.26 | $115.26 | To Order | Managed. Supports VLANs. |
| Patch Panel | [TRENDnet TC-KP24 24-Port Blank Keystone 1U](https://www.newegg.com/trendnet-tc-kp24-other/p/N82E16833980108) | 1 | $26.34 | $26.34 | To Order | Panel only — Cat6 cables sold separately. |
| Rack Enclosure | — | 1 | Owned | $0.00 | Owned | Existing rack. |
| UPS | — | — | — | — | Deferred | Revisit after initial build. |

**Shared / Rack Total: $831.57**

---

## Desktop Conversion (Gaming PC)

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| GPU Upgrade | [ASUS TUF Gaming RTX 4070 Super OC — Micro Center](https://www.microcenter.com/product/676134/asus-nvidia-geforce-rtx-4070-super-tuf-gaming-rgb-overclocked-triple-fan-12gb-gddr6x-pcie-40-graphics-card) | 1 | $226.10 | $226.10 | To Order | **Recommended over ProArt** — same price ($226.10), better cooling for a gaming rig. ProArt also available at same price if preferred. Card is 266mm — confirmed fits RSV-L4500U. |
| Rack Chassis | Listed in Shared / Rack | — | — | — | — | — |

> **GPU Note:** Original Amazon listing (ASUS ProArt RTX 4070 Super OC) was budgeted at $700. Both the ProArt and TUF Gaming OC are **$226.10 at Micro Center** — a saving of **$473.90**. The TUF Gaming OC is recommended for a gaming desktop (superior triple-fan cooling, lower noise). The ProArt is the better choice only if workstation color accuracy matters.

**Desktop Conversion Total: $226.10** *(chassis listed in Shared / Rack)*

---

## NAS Node

**OS:** TrueNAS SCALE
**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| CPU | [Intel Core i3-12100 — Newegg](https://www.newegg.com/intel-core-i3-12th-gen-core-i3-12100-alder-lake-lga-1700-desktop-cpu-processor/p/N82E16819118370) | 1 | $180.18 | $180.18 | To Order | LGA1700. Not stocked at Micro Center — check Amazon for better pricing. |
| Motherboard | [ASRock B760 Pro RS DDR5 — Newegg](https://www.newegg.com/p/N82E16813162103) | 1 | $179.99 | $179.99 | To Order | LGA1700, DDR5, 6x SATA ports. |
| Memory | [Crucial 16GB (2x8GB) DDR5-4800 — Micro Center](https://www.microcenter.com/product/643705/crucial-16gb-(2-x-8gb)-ddr5-4800-pc5-38400-cl40-dual-channel-desktop-memory-kit-ct2k8g48c40u5-black) | 1 | $43.99 | $43.99 | To Order | Verify in-store vs. online availability — Micro Center price is significantly below current shortage pricing. |
| Power Supply | [Corsair RM750e 750W 80+ Gold — Newegg](https://www.newegg.com/corsair-rmx-series-atx-3-1-compatible-750-w-cybenetics-gold-power-supply-black-rm750e/p/N82E16817139339) | 1 | $89.99 | $89.99 | To Order | **Upgraded from RM650e** — RM750e is $15 *cheaper* than the RM650e (which is back-ordered at $104.99) and provides more headroom. |
| Storage (HDD) | [Seagate IronWolf 12TB NAS HDD — Newegg](https://www.newegg.com/seagate-ironwolf-st12000vn0008-12tb/p/1JW-001N-00027) | 4 | $299.99 | $1,199.96 | To Order | ZFS RAID-Z1 (~36TB usable). Down from $369/drive — $276 total savings. |

**NAS Node Total: $1,694.11**

---

## App Server

**OS:** Ubuntu Server LTS
**Boot Device:** USB thumb drive (owned)

| Component | Model | Qty | Unit Price | Total | Status | Notes |
| :--- | :--- | :---: | ---: | ---: | :--- | :--- |
| CPU | [Intel Core i5-13500 — Newegg](https://www.newegg.com/intel-core-i5-13th-gen-core-i5-13500-raptor-lake-lga-1700-desktop-cpu-processor/p/N82E16819118429) | 1 | $298.99 | $298.99 | To Order | LGA1700, 14 cores / 20 threads. |
| Motherboard | [Gigabyte Z790 UD AX — Newegg](https://www.newegg.com/gigabyte-z790-ud-ax-atx-motherboard-intel-z790-lga-1700/p/N82E16813145436) | 1 | $268.18 | $268.18 | To Order | LGA1700, DDR5, PCIe 5.0. Supports GPU passthrough. |
| Memory | [Corsair VENGEANCE RGB 32GB DDR5-6000 — Micro Center](https://www.microcenter.com/product/688526/corsair-vengeance-rgb-32gb-(2-x-16gb)-ddr5-6000-pc5-48000-cl36-dual-channel-desktop-memory-kit-cmh32gx5m2m6000z36-black) | 1 | $389.99 | $389.99 | To Order | DDR5 shortage pricing — slight drop from budgeted $411. |
| Power Supply | [Corsair RM750e 750W 80+ Gold — Newegg](https://www.newegg.com/corsair-rmx-series-atx-3-1-compatible-750-w-cybenetics-gold-power-supply-black-rm750e/p/N82E16817139339) | 1 | $89.99 | $89.99 | To Order | Down from budgeted $145. |
| Storage (NVMe) | [Samsung 980 PRO 2TB — Micro Center](https://www.microcenter.com/product/633342/samsung-980-pro-ssd-2tb-m2-nvme-interface-pcie-gen-4x4-internal-solid-state-drive-with-v-nand-3-bit-mlc-technology-(mz-v8p2t0b-am)) | 2 | $76.79 | $153.58 | To Order | **Significant savings** — budgeted $377/drive, Micro Center has them at $76.79. Verify in-store availability. |
| GPU | Nvidia RTX 2070 Super | 1 | Owned | $0.00 | Owned | Pulled from gaming desktop. Jellyfin NVENC transcoding. |

**App Server Total: $1,200.73**

---

## Open Items / Decisions Pending

| # | Item | Decision Needed |
| :--- | :--- | :--- |
| ~~1~~ | ~~Desktop rack chassis~~ | ~~Resolved~~ — GPU is 266mm, confirmed fits RSV-L4500U. |
| ~~2~~ | ~~UPS~~ | ~~Resolved~~ — deferred. Revisit after initial build. |
| ~~3~~ | ~~Switch~~ | ~~Resolved~~ — upgrading to TL-SG1024DE (managed). |
| 4 | Cat6 patch cables | Patch panel price ($26.34) is panel only — cables not included. Order separately. |
| 5 | GPU final selection | ProArt vs TUF Gaming OC — both $226.10 at Micro Center. TUF recommended for gaming desktop. |
| 6 | Crucial DDR5 availability | $43.99 at Micro Center is well below current shortage pricing — verify in-store stock before relying on this price. |

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
