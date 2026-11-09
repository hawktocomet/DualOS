# DualOS GBL Android target and boot path source inspection

Report ID: `20261109T021639Z-dualos-gbl-android-target-and-boot-path-source-i`

Exit code: `0`

Command: `bash -c 
echo "=== TARGET SELECTION DEFINITION ==="
rg -n "get_target_os|enum TargetOs|TargetOs::Android" gbl

echo
echo "=== EFI MAIN BOOT FLOW ==="
sed -n "90,190p" gbl/efi/src/lib.rs

echo
echo "=== ANDROID BOOT IMPLEMENTATION REFERENCES ==="
rg -n "efi_android_load|efi_android_boot|android_boot|load.*android|boot.*android" \
  gbl/efi/src gbl/libefi/src

echo
echo "=== ANDROID PARTITION EXPECTATIONS ==="
rg -n "boot_a|boot_b|vendor_boot|vbmeta|super|metadata|misc|init_boot|dtbo" \
  gbl/efi/src gbl/libefi/src
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
