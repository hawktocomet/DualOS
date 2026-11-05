# Retroid Pocket Duo Lite Hardware Notes

## Platform identity

- SoC family: Qualcomm SM6125.
- GPU: Adreno 610.
- Device-tree compatible: `retroidpocket,duolite`, `qcom,sm6125`.
- GPU device-tree node: `gpu@5900000`.
- GPU firmware reference: `qcom/sm6125/a610_zap.mbn`.

## Boot and operating-system status

Verified on the actual Duo Lite:
- ROCKNIX ABL is installed.
- ROCKNIX has successfully booted from microSD.
- The project target remains a removable-storage-rooted system with stock Android preserved on internal storage.

This supersedes earlier repository wording that no alternative OS boot had been demonstrated.

A successful ROCKNIX boot is a platform milestone, not proof that a complete DualOS image, installer, update mechanism, or recovery process is ready. Do not modify internal Android storage as part of the intended product architecture.

## GPU devfreq

The Duo Lite GPU devfreq path observed in the audited ROCKNIX environment is:

`/sys/devices/platform/soc@0/5900000.gpu/devfreq/5900000.gpu`

The Duo Lite device tree declares these GPU operating points:

| Frequency |
|---:|
| 320 MHz |
| 465 MHz |
| 600 MHz |
| 745 MHz |
| 820 MHz |
| 900 MHz |
| 950 MHz |

ROCKNIX includes a userspace helper that writes `max_freq` through the devfreq interface. Its startup profile uses Bash array syntax, and the audited image's `/bin/sh` points to Bash.

The ROCKNIX helper also offers 980 MHz and 1050 MHz settings. Those exceed the highest GPU operating point declared in the audited Duo Lite device tree and must not be treated as verified or supported DualOS settings.

## Runtime and thermal investigation

The latest detailed fan/thermal investigation was performed on ROCKNIX 20261101, kernel Linux SM6115 7.2.0.

Observed runtime interfaces:
- pwmfan hwmon13.
- PWM controls: `pwm1`, `pwm1_enable`.
- Tachometer: `fan1_input`.
- Thermal cooling device4 corresponds to pwm-fan.
- Device-tree cooling levels: `[0, 102, 125, 140, 154, 169, 191]` on a 0-255 scale.
- Fan period: 20,000 ns.
- Tachometer: 2 pulses/revolution.
- Fan-related thermal trip points observed at 45, 50, 60, 70, 80, and 85 C.

Short low-temperature tests suggested lower PWM values are quieter:
- PWM 80: approximately 7,500 RPM, noisy.
- PWM 60: approximately 6,300 RPM, noisy.
- PWM 45: approximately 5,300 RPM, quiet.
- PWM 35: approximately 4,500 RPM, quieter.
- PWM 25: approximately 3,500 RPM, barely audible.
- PWM 20: approximately 3,000 RPM, inaudible while spinning.

These were short tests and are not sustained gaming validation. A 10-minute gaming baseline followed by a 60-second sample showed fan state 1 / PWM 102 at approximately 9,023 RPM and state 2 / PWM 125 at approximately 10,136 RPM; CPU peak was 51.2 C and GPU approximately 50 C.

No persistent fan curve changes were made during those tests. The fan was restored off and the controller enabled. Do not infer that low PWM values are safe for sustained gaming. Preserve all kernel thermal protections and validate under sustained load before proposing any persistent curve change.

## Display and input considerations

The Duo Lite has two physically distinct displays. Prior Android investigation showed different display characteristics and a synchronization issue under specific multitasking workloads.

Observed Android evidence:
- Setting `vendor.debug.egl.swapinterval=1` reproduced bottom-screen game stutter and audio crackle while top-screen activity was present.
- Setting it to 0 or disabling RetroArch VSync eliminated the symptom in the tested setup.
- Perfetto showed RetroArch dequeueBuffer waits of approximately 28-44 ms.
- SurfaceFlinger pacesetter display ID: `4630947073007009410`.
- Top display: 1080x1920, port 130, QCM, rotation 90.
- Bottom display: 1280x960, port 4, GH7005 ANX.
- Bottom display was initially a VSync-disallowed follower.

This evidence motivates careful HWC/compositor/frame-pacing validation. It does not establish the root cause of Linux display behavior.

## Initial DualOS hardware requirements

1. Discover the GPU devfreq node rather than assuming a fixed path.
2. Read available frequencies and current limits.
3. Only request frequencies exposed by the running kernel.
4. Preserve thermal throttling and kernel thermal policy.
5. Separate GPU clock policy from Vulkan/Turnip configuration.
6. Provide a conservative automatic profile by default.
7. Validate both displays independently, including orientation, touch mapping, composition, synchronization, and frame pacing.
8. Validate physical controls, audio, Wi-Fi, Bluetooth, battery, charging, and suspend/resume on the actual device.

## Evidence references and limitations

Detailed source reports, device captures, and historical investigations are maintained in the private [duo-lite-reports](https://github.com/hawktocomet/duo-lite-reports) repository.

The observations above are tied to the specific audited builds and test conditions. Recheck runtime paths, frequencies, temperatures, and kernel interfaces against the exact image under test. Static device-tree declarations do not guarantee that every operating point is available or stable at runtime.
