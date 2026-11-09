# Inventory Duo Lite Firehose recovery kit inputs

Report ID: `20261109T030134Z-inventory-duo-lite-firehose-recovery-kit-inputs`

Exit code: `0`

Command: `bash -lc 
BASE="/home/hawk"
CHAIN="$BASE/duo_forensics/android_boot_chain_20261102_094334"
EDL="$BASE/duo_edl/edl"
LOADER="$BASE/duo_forensics/qcs6125_loader_test/001750e100000000_d40eee56f3194665574109a39267724ae7944134cd53cb767e293d3c40497955bc8a4519ff992b031fadc6355015ac87.bin"

echo "========== FIREHOSE CLIENT =========="
ls -lh "$EDL/edl.py" "$EDL/.venv/bin/edl" "$EDL/edlclient/firehose" 2>/dev/null
echo
grep -nE "version|__version__" "$EDL/edl.py" "$EDL/edlclient/__init__.py" 2>/dev/null | head -n 20

echo
echo "========== EXACT PROGRAMMER LOADER =========="
if [ -f "$LOADER" ]; then
  stat -c "%n %s bytes" "$LOADER"
  sha256sum "$LOADER"
else
  echo "ERROR: known loader missing"
fi

echo
echo "========== RAW FIREHOSE PARTITION IMAGES =========="
find "$CHAIN" -maxdepth 2 -type f \
  \( -name "*.img" -o -name "*.bin" \) \
  -printf "%s\t%p\n" | sort -n

echo
echo "========== SUPER EXTRACTION OUTPUTS =========="
find "$CHAIN/super-unpacked-longopt" -type f \
  -printf "%s\t%p\n" 2>/dev/null | sort -n

echo
echo "========== FIREHOSE LOGS AND GPT INVENTORY =========="
find "$CHAIN" -maxdepth 2 -type f \
  \( -iname "*edl*log*" -o -iname "*gpt*" -o -iname "*partition*log*" -o -iname "*read*log*" \) \
  -printf "%s\t%p\n" | sort -n

echo
echo "========== AVAILABLE SPACE =========="
df -h "$BASE"

echo
echo "INVENTORY ONLY: no files modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
