# Retroid Pocket Duo Lite Hardware Notes

## Platform identity

- SoC family: Qualcomm SM6125
- GPU: Adreno 610
- Device tree compatible: `retroidpocket,duolite`, `qcom,sm6125`
- GPU device-tree node: `gpu@5900000`
- GPU firmware reference: `qcom/sm6125/a610_zap.mbn`

## ROCKNIX reference findings

Audited image: ROCKNIX SM6115 build dated 2026-11-01.

The image includes a Duo Lite-specific device tree. Its GPU devfreq path is:

`/sys/devices/platform/soc@0/5900000.gpu/devfreq/5900000.gpu`

The device tree declares these GPU operating points:

| Frequency |
|---:|
| 320 MHz |
| 465 MHz |
| 600 MHz |
| 745 MHz |
| 820 MHz |
| 900 MHz |
| 950 MHz |

ROCKNIX has a userspace helper that writes `max_freq` through this devfreq interface. Its startup profile uses Bash array syntax and its `/bin/sh` points to Bash.

The ROCKNIX helper also offers 980 MHz and 1050 MHz settings. Those values exceed the highest GPU operating point declared in the audited Duo Lite device tree. They are not accepted as verified DualOS settings.

## Initial DualOS requirements

1. Discover the GPU devfreq node rather than assuming a fixed path.
2. Read available frequencies and current limits.
3. Only request frequencies exposed by the running kernel.
4. Preserve thermal throttling and kernel thermal policy.
5. Separate GPU clock policy from Vulkan/Turnip configuration.
6. Provide a conservative automatic profile by default.

## Evidence limitations

These findings come from static inspection of a downloaded ROCKNIX image. They do not establish that the image boots on the Duo Lite, that every listed operating point is available at runtime, or that a requested frequency is successfully applied.

No handheld or microSD was accessed during this audit.

## Research reports

The detailed source reports are maintained separately in the private `duo-lite-reports` repository.
