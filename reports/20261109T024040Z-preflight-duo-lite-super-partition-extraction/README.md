# Preflight Duo Lite super partition extraction

Report ID: `20261109T024040Z-preflight-duo-lite-super-partition-extraction`

Exit code: `0`

Command: `bash -lc 
EDL="/home/hawk/duo_edl/edl/.venv/bin/edl"
ROOT="/home/hawk/duo_edl/edl"
LOADER="/home/hawk/duo_forensics/qcs6125_loader_test/001750e100000000_d40eee56f3194665574109a39267724ae7944134cd53cb767e293d3c40497955bc8a4519ff992b031fadc6355015ac87.bin"
OUT="/home/hawk/duo_forensics/android_boot_chain_20261102_094334/super.img"

echo "========== CLIENT HELP =========="
"$EDL" --help 2>&1 | head -n 180

echo
echo "========== MEMORY TYPE SUPPORT =========="
grep -RniE -m 30 "memory.*(emmc|ufs)|emmc.*memory|choices=.*memory|memory_type" \
  "$ROOT/edlclient" "$ROOT/edl.py" 2>/dev/null

echo
echo "========== LOADER =========="
if [ -f "$LOADER" ]; then
  ls -lh "$LOADER"
  sha256sum "$LOADER"
else
  echo "ERROR: expected loader missing"
fi

echo
echo "========== OUTPUT CAPACITY =========="
mkdir -p "$(dirname "$OUT")"
df -h "$(dirname "$OUT")"
if [ -e "$OUT" ]; then
  echo "WARNING: output already exists; it will not be overwritten"
  ls -lh "$OUT"
else
  echo "Output path is clear: $OUT"
fi

echo
echo "========== EXPECTED READ =========="
echo "LUN: 0"
echo "Start sector: 3824640"
echo "Sector count: 25165824"
echo "Sector size: 512 bytes"
echo "Expected image size: 12884901888 bytes (12 GiB)"
echo "PREFLIGHT ONLY: no USB/device access and no partition read"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
