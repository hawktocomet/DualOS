# Extract Duo Lite Android Virtual A/B super slots

Report ID: `20261109T025432Z-extract-duo-lite-android-virtual-a-b-super-slots`

Exit code: `0`

Command: `bash -lc 
IMG="/home/hawk/duo_forensics/android_boot_chain_20261102_094334/super.img"
ROOT="/home/hawk/duo_forensics/android_boot_chain_20261102_094334"
OUT="$ROOT/super-unpacked-longopt"
A="$OUT/slot0"
B="$OUT/slot1"

echo "========== PRE-FLIGHT =========="
if [ ! -f "$IMG" ]; then echo "ERROR: super.img missing"; exit 20; fi
if [ -e "$OUT" ]; then echo "ERROR: output path already exists; refusing overwrite"; exit 21; fi
if [ "$(stat -c%s "$IMG")" -ne 12884901888 ]; then echo "ERROR: unexpected super.img size"; exit 22; fi
df -h "$ROOT"

mkdir -p "$A" "$B"

echo
echo "========== EXTRACT SLOT 0 =========="
lpunpack --slot=0 "$IMG" "$A"
RC=$?
echo "Slot 0 exit code: $RC"
if [ "$RC" -ne 0 ]; then echo "ERROR: slot 0 extraction failed; preserving output"; exit "$RC"; fi

echo
echo "========== EXTRACT SLOT 1 =========="
lpunpack --slot=1 "$IMG" "$B"
RC=$?
echo "Slot 1 exit code: $RC"
if [ "$RC" -ne 0 ]; then echo "ERROR: slot 1 extraction failed; preserving output"; exit "$RC"; fi

echo
echo "========== EXTRACTED FILES =========="
find "$OUT" -maxdepth 2 -type f -printf "%P %s bytes\n" | sort

echo
echo "========== DIRECTORY SIZES =========="
du -sh "$A" "$B"

echo
echo "========== ORIGINAL IMAGE =========="
stat -c "%n %s bytes" "$IMG"
sha256sum "$IMG"

echo
echo "========== RESULT =========="
echo "SUPER_SLOT_EXTRACTION_COMPLETE"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
