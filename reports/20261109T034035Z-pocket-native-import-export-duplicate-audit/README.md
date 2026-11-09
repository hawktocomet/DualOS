# pocket-native-import-export-duplicate-audit

Report ID: `20261109T034035Z-pocket-native-import-export-duplicate-audit`

Exit code: `0`

Command: `bash -c 
ROOT="$HOME/source/pocket-recompiler/target"

echo "========== IMPORT / EXPORT DIRECTORY SIZES =========="
for d in \
  "$ROOT/pocket-native-import" \
  "$ROOT/pocket-native-fastmem-import" \
  "$ROOT/pocket-native-export"
do
  echo "--- $d"
  du -sh "$d" 2>/dev/null
  du -h --max-depth=2 "$d" 2>/dev/null | sort -h | tail -n 20
done

echo
echo "========== GAME IMAGE HASHES =========="
find "$ROOT/pocket-native-import" \
     "$ROOT/pocket-native-fastmem-import" \
     "$ROOT/pocket-native-export" \
     -type f -name game.3ds -print0 2>/dev/null |
while IFS= read -r -d "" f; do
  sha256sum "$f"
done

echo
echo "========== GENERATED OUTPUT INVENTORY =========="
find "$ROOT/pocket-native-import" \
     "$ROOT/pocket-native-fastmem-import" \
     "$ROOT/pocket-native-export" \
     -type f ! -name game.3ds -printf "%s\t%p\n" 2>/dev/null |
sort -nr | head -n 60 |
numfmt --field=1 --to=iec --suffix=B

echo
echo "========== EXPORT PACKAGE CONTENTS =========="
find "$ROOT/pocket-native-export" -maxdepth 4 -printf "%y\t%p\n" 2>/dev/null |
head -n 120

echo
echo "========== TOP-LEVEL TEST ARTIFACTS =========="
ls -lah "$ROOT" | head -n 80

echo
echo "READ-ONLY: no files modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
