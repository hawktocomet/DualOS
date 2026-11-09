# Extract Duo Lite Android Virtual A/B super slots

Report ID: `20261109T025254Z-extract-duo-lite-android-virtual-a-b-super-slots`

Exit code: `64`

Command: `bash -lc 
IMG="/home/hawk/duo_forensics/android_boot_chain_20261102_094334/super.img"
ROOT="/home/hawk/duo_forensics/android_boot_chain_20261102_094334"
A="$ROOT/super-unpacked/slot0"
B="$ROOT/super-unpacked/slot1"
EXPECTED=12884901888

echo "========== PRE-EXTRACTION CHECKS =========="
if [ ! -f "$IMG" ]; then echo "ERROR: super.img missing"; exit 20; fi
if [ "$(stat -c%s "$IMG")" -ne "$EXPECTED" ]; then echo "ERROR: source image size mismatch"; exit 21; fi
if [ -e "$A" ] || [ -e "$B" ]; then echo "ERROR: output directory already exists; refusing to overwrite"; exit 22; fi
if ! command -v lpunpack >/dev/null 2>&1; then echo "ERROR: lpunpack unavailable"; exit 23; fi

mkdir -p "$A" "$B"

echo
echo "========== EXTRACT METADATA SLOT 0 =========="
lpunpack -S 0 "$IMG" "$A"
RC=$?
echo "Slot 0 exit code: $RC"
if [ "$RC" -ne 0 ]; then echo "ERROR: slot 0 extraction failed; preserving partial output"; exit "$RC"; fi

echo
echo "========== EXTRACT METADATA SLOT 1 =========="
lpunpack -S 1 "$IMG" "$B"
RC=$?
echo "Slot 1 exit code: $RC"
if [ "$RC" -ne 0 ]; then echo "ERROR: slot 1 extraction failed; preserving partial output"; exit "$RC"; fi

echo
echo "========== EXTRACTED FILE INVENTORY =========="
find "$ROOT/super-unpacked" -maxdepth 2 -type f -printf "%P %s bytes\n" | sort

echo
echo "========== SLOT DIRECTORY SIZES =========="
du -sh "$A" "$B"

echo
echo "========== SOURCE IMAGE STILL INTACT =========="
stat -c "%n %s bytes" "$IMG"
sha256sum "$IMG"

echo
echo "========== REMAINING SPACE =========="
df -h "$ROOT"

echo
echo "SUPER_SLOT_EXTRACTION_COMPLETE"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
