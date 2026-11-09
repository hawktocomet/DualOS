# Read Duo Lite Android super partition to image

Report ID: `20261109T024128Z-read-duo-lite-android-super-partition-to-image`

Exit code: `0`

Command: `bash -lc 
EDL="/home/hawk/duo_edl/edl/.venv/bin/edl"
LOADER="/home/hawk/duo_forensics/qcs6125_loader_test/001750e100000000_d40eee56f3194665574109a39267724ae7944134cd53cb767e293d3c40497955bc8a4519ff992b031fadc6355015ac87.bin"
OUT="/home/hawk/duo_forensics/android_boot_chain_20261102_094334/super.img"
EXPECTED=12884901888

echo "========== FINAL SAFETY CHECKS =========="
if [ ! -x "$EDL" ]; then echo "ERROR: EDL client unavailable"; exit 20; fi
if [ ! -f "$LOADER" ]; then echo "ERROR: programmer loader unavailable"; exit 21; fi
if [ -e "$OUT" ]; then echo "ERROR: output already exists; refusing to overwrite"; exit 22; fi

AVAILABLE=$(df -B1 --output=avail "$(dirname "$OUT")" | tail -n 1 | tr -d " ")
echo "Available bytes: $AVAILABLE"
if [ "$AVAILABLE" -lt "$EXPECTED" ]; then echo "ERROR: insufficient free space"; exit 23; fi

echo
echo "========== READ-ONLY FIREHOSE EXTRACTION =========="
echo "Operation: rs (read sectors)"
echo "Memory: eMMC"
echo "LUN: 0"
echo "Start sector: 3824640"
echo "Sector count: 25165824"
echo "Sector size: 512"
echo "Destination: $OUT"
echo

"$EDL" rs 3824640 25165824 "$OUT" \
  --lun=0 \
  --sectorsize=512 \
  --memory=eMMC \
  --loader="$LOADER"

RC=$?
echo
echo "Firehose read exit code: $RC"
if [ "$RC" -ne 0 ]; then
  echo "ERROR: read failed; any partial output has been preserved"
  [ -f "$OUT" ] && ls -lh "$OUT"
  exit "$RC"
fi

echo
echo "========== IMAGE VALIDATION =========="
if [ ! -f "$OUT" ]; then echo "ERROR: no output image created"; exit 24; fi
ACTUAL=$(stat -c%s "$OUT")
echo "Expected bytes: $EXPECTED"
echo "Actual bytes:   $ACTUAL"
ls -lh "$OUT"
if [ "$ACTUAL" -ne "$EXPECTED" ]; then
  echo "ERROR: image size mismatch; preserving output for inspection"
  exit 25
fi

sha256sum "$OUT"
echo "SUPER_EXTRACTION_VERIFIED"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
