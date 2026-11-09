# Inspect Duo Lite Android super dynamic partition metadata

Report ID: `20261109T025157Z-inspect-duo-lite-android-super-dynamic-partition`

Exit code: `0`

Command: `bash -lc 
IMG="/home/hawk/duo_forensics/android_boot_chain_20261102_094334/super.img"
EXPECTED=12884901888

echo "========== IMAGE IDENTITY =========="
if [ ! -f "$IMG" ]; then echo "ERROR: super.img missing"; exit 20; fi
ACTUAL=$(stat -c%s "$IMG")
echo "Size: $ACTUAL bytes"
echo "Expected: $EXPECTED bytes"
file "$IMG"
if [ "$ACTUAL" -ne "$EXPECTED" ]; then echo "ERROR: size mismatch"; exit 21; fi

echo
echo "========== AVAILABLE ANDROID PARTITION TOOLS =========="
for TOOL in lpdump lpunpack simg2img unpack_bootimg avbtool; do
  if command -v "$TOOL" >/dev/null 2>&1; then
    printf "%-16s %s\n" "$TOOL" "$(command -v "$TOOL")"
  else
    printf "%-16s not in PATH\n" "$TOOL"
  fi
done

echo
echo "========== TOOL PACKAGE LOCATIONS =========="
for ROOT in \
  /home/hawk/Android/Sdk \
  /opt/android-sdk \
  /usr/lib/android-sdk \
  /home/hawk/.local/bin
do
  if [ -d "$ROOT" ]; then
    find "$ROOT" -maxdepth 6 -type f \
      \( -name lpdump -o -name lpunpack -o -name simg2img \) \
      -print 2>/dev/null
  fi
done

echo
echo "========== DYNAMIC PARTITION METADATA =========="
if command -v lpdump >/dev/null 2>&1; then
  lpdump "$IMG" 2>&1
else
  echo "lpdump unavailable; no unpacking attempted"
fi

echo
echo "========== FIRST 4 KiB SIGNATURE / HEADER =========="
xxd -l 4096 "$IMG" | head -n 24

echo
echo "INSPECTION ONLY: super.img was not modified or unpacked"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
