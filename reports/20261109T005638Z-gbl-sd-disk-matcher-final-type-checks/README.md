# GBL SD disk matcher final type checks

Report ID: `20261109T005638Z-gbl-sd-disk-matcher-final-type-checks`

Exit code: `0`

Command: `bash -c 
cd bootable/libbootloader || exit 1

echo "===== DEVICE HANDLE DEFINITION ====="
rg -n -B 8 -A 35 "pub struct DeviceHandle|struct DeviceHandle" gbl/libefi/src/lib.rs

echo
echo "===== ERROR VARIANTS ====="
rg -n "enum Error|NotFound|NotUnique|InvalidInput|InvalidState" gbl/liberror

echo
echo "===== COMPLETE BLOCK DISCOVERY FUNCTION ====="
sed -n "115,230p" gbl/efi/src/efi_blocks.rs

echo
echo "===== ORIGINAL BOOT ENTRYPOINT ====="
git show HEAD:gbl/efi/src/lib.rs | sed -n "45,180p"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
