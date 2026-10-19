# device-<codename>

Contributor tracker for one Omnipha device. Hardware and package facts stay in [`ports`](https://github.com/omnipha/ports). This repository holds device issues, tests, flashing notes, and links. It never holds a kernel tree, firmware blobs, or secrets.

| Fact | Value |
| --- | --- |
| Codename | `xiaomi-laurel` |
| Name | Xiaomi Mi A3 |
| Vendor | Xiaomi |
| SoC | `sm6125` |
| Status | boot=works, ssh=works, wifi=works |
| Device entry | [ports/devices/xiaomi-laurel](https://github.com/omnipha/ports/tree/main/devices/xiaomi-laurel) |
| Kernel | [omnipha/linux-sm61x5](https://github.com/omnipha/linux-sm61x5) branch `omnipha` |
| Packages | [omnipha/ports](https://github.com/omnipha/ports) |
| Images | [omnipha/images](https://github.com/omnipha/images) |

## Reproduce

1. Read `devices/xiaomi-laurel/device.yaml` in `ports` and confirm the reported status.
2. Build only from the pinned `images` definition. Do not copy `omnipha/linux-sm61x5` into this repo.
3. Flash with the method named in that device entry, then record boot, network, and SSH on real hardware.
4. File the result here. Package changes go to `ports`; kernel changes go to `omnipha/linux-sm61x5`; image changes go to `images`. Link the cross-repo issue or PR in both directions.

## Ownership

- `ports` owns the device database and packages.
- `omnipha/linux-sm61x5` owns shared kernel implementation. It is a mirror plus Omnipha patches, not an issue tracker.
- `images` owns reproducible build definitions.
- This repo owns device-specific issues and tests. A device supervisor is registered separately and is not added to the main-host allowlist.
- Creating this repo does not assign or claim its issues.

## Teardown

Archive this repository when the device is retired or replaced. Do not delete issue history. Remove the `device_repo` link from `ports` in the same change, and leave kernel, package, and image history intact.
