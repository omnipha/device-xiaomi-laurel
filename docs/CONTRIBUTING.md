# Contributing

This document is a guide for honest contributions to `device-xiaomi-laurel`.
The overarching rule:

> If you cannot show *evidence* (log, dmesg, command transcript, photo of the
> unit), do not file it as a *result*. "Untested" is a useful, citable status.
> Don't paper over it by writing "works".

## Files of record

- Status (works / partial / broken / untested):
  [`omnipha/ports/devices/xiaomi-laurel/device.yaml`](https://github.com/omnipha/ports/blob/main/devices/xiaomi-laurel/device.yaml)
- Image definition & flash plan:
  [`omnipha/images/devices/xiaomi-laurel/image.yaml`](https://github.com/omnipha/images/blob/main/devices/xiaomi-laurel/image.yaml)
- Kernel: [`omnipha/linux-sm61x5`](https://github.com/omnipha/linux-sm61x5), branch `omnipha`
- Issues & per-unit evidence: **this repo**.

## Filing an issue

For an open bring-up item (broken cameras, untested audio, etc.) the issue
template is good enough — pick the right tracker.

For a *new* observation (e.g. "wifi stops after suspend on my unit"):

1. Open an issue here with labels `in-progress` and the matching `prio:`
   label from [`labels.json`](../labels.json).
2. Include:
   - **device variant** — `4/64` or `4/128` (`device.yaml` `variants:`).
   - **firmware version on the unit** — Android 9 or Android 11.
   - **exact commands run** and a paste of the output. Don't paraphrase.
   - **`dmesg` snippet** for kernel events (use `dmesg -T` and trim).
   - **kernel commit / tag** you were on (`linux-sm61x5` `omnipha` branch).
3. Do **not** add credentials, MAC addresses, IMEI, serial numbers, or
   APKs/firmware blobs. Redact everything that is unique to your unit.

## Promoting "untested" → "works" (or "broken")

A status flip is a *cross-repo change*. The correct sequence is:

1. Open an issue here with your evidence (logs above).
2. Open a PR against `omnipha/ports` that updates
   `devices/xiaomi-laurel/device.yaml` in the same PR thread. The PR
   description must link the issue, the kernel tag, and the commands that
   produced the verdict.
3. If the flip depends on a kernel change, the kernel patch must **land
   first** in `omnipha/linux-sm61x5` on the `omnipha` branch (referenced
   from the ports PR).
4. If the flip requires a new package variant, land it in `ports` first too.
5. After merge, close the issue here with the exact merge commit SHA.

Don't edit the status table in this repo directly — it is documentation,
not authority.

## Promoting status in the other direction

If you find something that *was* working and now isn't, the same flow applies
in reverse: issue → PR that links it. Mark the issue `blocked` if a
concrete unblocker (kernel commit, firmware file, method change) is missing.

## Image & build process PRs

Until `omnipha/images` builds and publishes an artifact for this device,
*this repo will not* carry a flash runbook. If you have one to propose:

1. Open the PR against `omnipha/images/devices/xiaomi-laurel/image.yaml`
   (or the wider `images` repo).
2. The workflow `Validate image definitions` will lint it against a pinned
   `ports` SHA.
3. The flash procedure you describe must point at **upstream** postmarketOS /
   Ubuntu Touch (UBports) / OEM pages, not at a recipe you wrote from memory.
4. Once that PR is merged and the `BLOCKED:` line in
   `images/.github/workflows/build.yml` goes away, update this repo's README
   to point at it — not before.

## Fastboot instructions: do not invent them

Do not paste "fastboot oem unlock" / `flash userdata` / etc. into a PR or
issue here unless it is **your own transcript** with timestamps, command,
exit code, and unit serial (redacted as needed). Generic copy-pasted
instructions will be rejected — they have a habit of being wrong for one
firmware but not the other, and bricking a unit silently is the worst
possible failure mode for this project.

## Code of conduct

Be accurate. "Works" means "we tried it on real hardware and have logs".
"Partial" means "some part works, the rest is open". "Untested" is fine and
helpful — leave it as that until you have evidence.
