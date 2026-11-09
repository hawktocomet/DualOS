# Create focused Duo Lite Firehose and partition backup

Report ID: `20261109T030233Z-create-focused-duo-lite-firehose-and-partition-b`

Exit code: `0`

Command: `bash -lc 
BASE="/home/hawk"
EDL="$BASE/duo_edl/edl"
CHAIN="$BASE/duo_forensics/android_boot_chain_20261102_094334"
LOADER_DIR="$BASE/duo_forensics/qcs6125_loader_test"
DEST="$BASE/duo_forensics/duolite-firehose-backup"
LOADER="001750e100000000_d40eee56f3194665574109a39267724ae7944134cd53cb767e293d3c40497955bc8a4519ff992b031fadc6355015ac87.bin"

echo "========== PREFLIGHT =========="
if [ -e "$DEST" ]; then
  echo "ERROR: destination already exists; refusing overwrite"
  exit 20
fi

for P in "$EDL/edl.py" "$EDL/edlclient" "$EDL/.venv" "$LOADER_DIR/$LOADER" "$CHAIN/super.img"; do
  if [ ! -e "$P" ]; then
    echo "ERROR: required source missing: $P"
    exit 21
  fi
done

df -h "$BASE"
echo
echo "========== CREATE DIRECTORIES =========="
mkdir -p "$DEST/firehose-program" \
         "$DEST/programmer-loader" \
         "$DEST/duo-lite-captures/raw-partitions" \
         "$DEST/duo-lite-captures/extracted-logical-partitions" \
         "$DEST/duo-lite-captures/logs" \
         "$DEST/duo-lite-captures/additional-dumps"

echo
echo "========== COPY FIREHOSE PROGRAM =========="
cp -a --reflink=auto "$EDL/edl.py" "$DEST/firehose-program/"
cp -a --reflink=auto "$EDL/edlclient" "$DEST/firehose-program/"
cp -a --reflink=auto "$EDL/.venv" "$DEST/firehose-program/"
cp -a --reflink=auto "$EDL/.git" "$DEST/firehose-program/"

for F in README.md LICENSE setup.py setup.cfg pyproject.toml requirements.txt requirements-dev.txt; do
  if [ -f "$EDL/$F" ]; then
    cp -a "$EDL/$F" "$DEST/firehose-program/"
  fi
done

git -C "$EDL" rev-parse HEAD > "$DEST/firehose-program/SOURCE-COMMIT.txt"
"$EDL/.venv/bin/python" -m pip freeze > "$DEST/firehose-program/PYTHON-ENVIRONMENT.txt" 2>&1
echo "Firehose source, Git history and Python environment copied."

echo
echo "========== COPY EXACT PROGRAMMER LOADER =========="
cp -a "$LOADER_DIR/$LOADER" "$DEST/programmer-loader/"
cp -a "$LOADER_DIR/REPORT.txt" "$DEST/programmer-loader/" 2>/dev/null || true
sha256sum "$DEST/programmer-loader/$LOADER"

echo
echo "========== COPY ORIGINAL FIREHOSE PARTITION IMAGES =========="
find "$CHAIN" -maxdepth 1 -type f \
  \( -name "*.img" -o -name "*.bin" \) -print0 |
while IFS= read -r -d "" F; do
  cp -a --reflink=auto --sparse=always "$F" "$DEST/duo-lite-captures/raw-partitions/"
  echo "Copied $(basename "$F")"
done

echo
echo "========== COPY EXTRACTED A/B LOGICAL PARTITIONS =========="
cp -a --reflink=auto --sparse=always \
  "$CHAIN/super-unpacked-longopt/slot0" \
  "$CHAIN/super-unpacked-longopt/slot1" \
  "$DEST/duo-lite-captures/extracted-logical-partitions/"

echo
echo "========== COPY FIREHOSE CAPTURE LOGS =========="
for F in "$CHAIN/gpt_inventory.log" "$CHAIN/edl_partition_read.log"; do
  if [ -f "$F" ]; then
    cp -a "$F" "$DEST/duo-lite-captures/logs/"
  fi
done

echo
echo "========== COPY ADDITIONAL AUTHORIZED DUMPS =========="
for SRC in \
  "$BASE/duo_edl/edl/new_authorized_dumps" \
  "$BASE/duo_edl/new_authorized_dumps"; do
  if [ -d "$SRC" ]; then
    NAME=$(basename "$(dirname "$SRC")")-$(basename "$SRC")
    mkdir -p "$DEST/duo-lite-captures/additional-dumps/$NAME"
    cp -a --reflink=auto --sparse=always "$SRC/." \
      "$DEST/duo-lite-captures/additional-dumps/$NAME/"
    echo "Copied $SRC"
  fi
done

echo
echo "========== WRITE ARCHIVE README =========="
cat > "$DEST/README.txt" <<EOF
DUO LITE FIREHOSE BACKUP

firehose-program/
  B.Kerler EDL client source, Git history, Python virtual environment,
  and recorded Python package versions.

programmer-loader/
  Exact Qualcomm Firehose programmer loader used for the Duo Lite.

duo-lite-captures/raw-partitions/
  Original partition images read from the Duo Lite.

duo-lite-captures/extracted-logical-partitions/
  Logical Android A/B partitions extracted from super.img.
  These are derived files, not separate physical Firehose reads.

duo-lite-captures/logs/
  GPT inventory and Firehose partition-read log.

duo-lite-captures/additional-dumps/
  Additional authorized dump directories found during inventory.

Original source files were copied, not moved or deleted.
No device partitions were written or modified.

The copied virtual environment may retain its original absolute paths.
The source and dependency inventory are included for reproducibility.
EOF

echo
echo "========== BUILD INVENTORY =========="
find "$DEST" -type f -printf "%P\t%s bytes\n" | sort > "$DEST/FILE-INVENTORY.tsv"

echo
echo "========== BUILD SHA256 MANIFEST =========="
(
  cd "$DEST" || exit 30
  find . -type f ! -name SHA256SUMS -print0 |
    sort -z |
    xargs -0 -r sha256sum > SHA256SUMS
)
RC=$?
if [ "$RC" -ne 0 ]; then
  echo "ERROR creating checksum manifest"
  exit "$RC"
fi

echo
echo "========== VERIFY ARCHIVE =========="
(
  cd "$DEST" || exit 31
  sha256sum -c SHA256SUMS
)
RC=$?
if [ "$RC" -ne 0 ]; then
  echo "ERROR: verification failed"
  exit "$RC"
fi

echo
echo "========== VERIFY SUPER IMAGE =========="
sha256sum "$DEST/duo-lite-captures/raw-partitions/super.img"

echo
echo "========== FINAL SUMMARY =========="
du -sh "$DEST"
echo "Files: $(find "$DEST" -type f | wc -l)"
echo "Checksum entries: $(wc -l < "$DEST/SHA256SUMS")"
df -h "$BASE"
echo "Archive: $DEST"
echo "DUO_LITE_FIREHOSE_BACKUP_COMPLETE"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
