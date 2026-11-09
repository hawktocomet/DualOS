# Inspect extracted Duo Lite Android slots and snapshot formats

Report ID: `20261109T025607Z-inspect-extracted-duo-lite-android-slots-and-sna`

Exit code: `0`

Command: `bash -lc 
ROOT="/home/hawk/duo_forensics/android_boot_chain_20261102_094334"
OUT="$ROOT/super-unpacked-longopt"

echo "========== PREVIOUSLY CAPTURED ACTIVE SLOT =========="
if [ -f /home/hawk/duo_fastboot_vars.txt ]; then
  grep -iE "current-slot|slot-suffix|slot-count|unlocked|verifiedboot" /home/hawk/duo_fastboot_vars.txt
else
  echo "No saved fastboot variables file found"
fi

echo
echo "========== PARTITION IMAGE FORMATS =========="
find "$OUT" -type f -name "*.img" -print0 |
  sort -z |
  xargs -0 -r file

echo
echo "========== FILESYSTEM SIGNATURES =========="
for IMG in "$OUT"/slot0/{system,system_ext,product,vendor,odm,system_dlkm,vendor_dlkm}_a.img \
           "$OUT"/slot1/{system,system_ext,product,vendor,odm,system_dlkm,vendor_dlkm}_b.img; do
  [ -f "$IMG" ] || continue
  echo
  echo "--- $(basename "$IMG") ---"
  blkid -p "$IMG" 2>&1
  od -An -tx1 -N16 "$IMG"
done

echo
echo "========== COW IMAGE HEADERS =========="
find "$OUT" -type f -name "*-cow.img" -print0 |
  sort -z |
  while IFS= read -r -d "" IMG; do
    echo
    echo "--- ${IMG#$OUT/} ---"
    stat -c "%s bytes" "$IMG"
    od -An -tx1 -N64 "$IMG"
    strings -n 8 "$IMG" | head -n 8
  done

echo
echo "========== AVAILABLE SNAPSHOT UTILITIES =========="
for TOOL in inspect_cow cow_dump snapshotctl snapuserd lpdump lpunpack simg2img avbtool; do
  command -v "$TOOL" 2>/dev/null || true
done

echo
echo "========== SOURCE IMAGE HASH =========="
sha256sum "$ROOT/super.img"

echo
echo "INSPECTION ONLY: no images or device partitions modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
