# device-xiaomi-laurel (Xiaomi Mi A3)

Contributor tracker for one Omnipha device. Hardware and package facts live in
[`omnipha/ports`](https://github.com/omnipha/ports). This repository holds device
issues, tests, flashing notes, and links. It never holds a kernel tree, firmware
blobs, or secrets.

| Fact | Value |
| --- | --- |
| Codename | `xiaomi-laurel` |
| Name | Xiaomi Mi A3 |
| Vendor | Xiaomi |
| SoC | `sm6125` |
| Status (measured) | boot=works, ssh=works, wifi=works, display=works, gpu=works, ui=partial; modem=partial; cameras=untested; rest=untested |
| Canonical status | [`ports/devices/xiaomi-laurel/device.yaml`](https://github.com/omnipha/ports/blob/main/devices/xiaomi-laurel/device.yaml) |
| Kernel | [`omnipha/linux-sm61x5`](https://github.com/omnipha/linux-sm61x5) branch `omnipha` |
| Packages | [`omnipha/ports`](https://github.com/omnipha/ports) |
| Images | [`omnipha/images`](https://github.com/omnipha/images) |
| Image build | **No downloadable Omnipha image. CI is validate-only and prints a `BLOCKED` line — see below.** |
| Flash method (per `image.yaml`) | `fastboot` |

> This repo is honest about what we know. If you flash a unit and observe
> something different, file an issue with the evidence attached — see
> [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md).

## Measured hardware status

Source of truth for the status table above is
[`omnipha/ports/devices/xiaomi-laurel/device.yaml`](https://github.com/omnipha/ports/blob/main/devices/xiaomi-laurel/device.yaml).
This README only summarises it; **do not edit status here** — open a PR against
`ports` so every cross-repo project sees the same fact.

What we have actually proven, and how we know:

- **boot** — `status: works`. Kernel pinned to the `omnipha` branch of
  `linux-sm61x5` (base tag `v7.2.3-r0`).
- **ssh** — `status: works`. Key-only login verified on our unit.
- **wifi** — `status: works`. `device.yaml` records the WiFi patch as
  `omnipha` branch, WCN3990 brought up via MPSS remoteproc, **verified on
  2026-10-18**. This is *measured*, not assumed.
- **display / gpu** — `status: works`. Panel comes up as a UI output because
  `deviceinfo_drm="true"` is set in the `ports` variant. Adreno 610 reports
  `gpu-initialized: 1`.
- **ui** — `status: partial`. Weston is OK; Phosh boots without a lockscreen.
  Lockscreen / Phosh touch behaviour is not yet exercised.
- **modem** — `status: partial`. MPSS remoteproc starts; no call / data test yet.
  Tracked in [issue #8](https://github.com/omnipha/device-xiaomi-laurel/issues/8).
- **cameras** — `status: untested`. Camera bring-up is open, but the
  [camera issue #1](https://github.com/omnipha/device-xiaomi-laurel/issues/1)
  contains no measured camera test. Do not infer a driver defect or working
  camera until a test is recorded.

Things we explicitly have **not** measured yet (i.e. “untested”, not broken):

`cameras`, `audio`, `bluetooth`, `sensors`, `gps`, `nfc`, `touchscreen`.
Tracker issues #1–#7 describe test steps. Do **not** mark them `works`,
`partial` or `broken` until a specific observation has a public test record.

> `broken` requires an observed failing test. An open bring-up issue is not
> evidence of failure. See the evidence-gated status rules in
> [ports/CONTRIBUTING.md](https://github.com/omnipha/ports/blob/main/CONTRIBUTING.md).

## No downloadable Omnipha image (yet)

`ports/devices/xiaomi-laurel/device.yaml` declares:

```yaml
release:
  status: unreleased
upstream_os:
  nura:
    status: self-built
    image: none
```

In `omnipha/images/.github/workflows/build.yml` the validation job ends with:

> BLOCKED: omnipha/builder lacks `--non-interactive`, `--profile`,
> `--ssh-pubkey` and `image.yaml` support; no image was built.

So today there is **no flashable Omnipha image for this device**. Anything you
read on the web claiming otherwise is not from this project. Don't link to it;
if you find one, open an issue.

### Building, the moment a real procedure exists

We intentionally do not publish a build runbook here until `images` actually
produces an artifact end-to-end. Concretely, the procedure will be linked from the published `ports` release evidence and the `images`
release, with pinned inputs and device-specific rollback instructions. The
[`image.yaml`](https://github.com/omnipha/images/blob/main/devices/xiaomi-laurel/image.yaml)
remains an experimental definition, not an executable guide. This README will be extended to
reference the real procedure when one exists — not before.

Until then, the only “reproduce” step that's safe to run is reading
`device.yaml` and confirming the status above matches your hardware.

## Flashing — do not invent device-specific steps

`ports` and `images` both say `flash.method: fastboot`. **This README does not
document a Mi A3 fastboot runbook** because:

1. A generic upstream flasher flow is not a reviewed Mi A3 rollback-safe
   installation procedure.
2. Inheriting a copy-paste of `fastboot oem unlock` / bootloader sequence is
   easy to get wrong and could brick units.
3. Some Mi A3 units are on Android 11 firmware, which the UBports installer
   flags as “dangerous to roll back to Android 9” (see
   `device.yaml` `upstream_os.ut.note`). That decision belongs in `ports`, not
  in this tracker.

If you have a tested procedure to propose, open an issue with a redacted
command transcript, tested firmware variant and verified rollback path. Link
upstream and OEM documentation. Do not ask others to flash from a draft.

## Cross-repo routing (where work goes)

| Kind of change | Goes to |
| --- | --- |
| Status flip, package variant, kernel patch pinned here | [`omnipha/ports`](https://github.com/omnipha/ports) (`devices/xiaomi-laurel/`) |
| Shared kernel work, WiFi remoteproc patch, etc. | [`omnipha/linux-sm61x5`](https://github.com/omnipha/linux-sm61x5) branch `omnipha` |
| Image / build definition, flash plan | [`omnipha/images`](https://github.com/omnipha/images) (`devices/xiaomi-laurel/image.yaml`) |
| Per-unit flashing log, test result, new observation | **Here** (issue with evidence), plus a cross-link back from the `ports` / `linux-sm61x5` / `images` PR. |

When you open a PR in any of those other repos, link the matching issue in
this repo (and vice versa). Do not duplicate the same change in two repos.

## What this repo does **not** do

- Doesn't hold kernel code, firmware blobs, or secrets.
- Doesn't auto-claim its issues — a device supervisor is registered separately
  and is **not** added to the main-host allowlist (see `AGENTS.md`).
- Doesn't promise a flashable image at any specific date.
- Doesn't carry unwritten fastboot instructions.

## Ownership

- `ports` — device database, status table, packages.
- `omnipha/linux-sm61x5` — shared kernel implementation. It is a mirror plus
  Omnipha patches; it is not an issue tracker.
- `images` — reproducible build definitions. Currently validate-only for this
  device.
- This repo — device-specific issues, tests, links.

## Teardown

Archive this repository when the device is retired or replaced. Do not delete
issue history. Remove the `device_repo` link from `ports` in the same change,
and leave kernel, package, and image history intact.
