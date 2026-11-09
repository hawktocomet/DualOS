# pocket-native-android-clang-builder-audit

Report ID: `20261109T033834Z-pocket-native-android-clang-builder-audit`

Exit code: `0`

Command: `bash -c 
ROOT="$HOME/source/pocket-recompiler/target/android-clang-builder"

echo "========== BUILDER BREAKDOWN =========="
du -xhd3 "$ROOT" 2>/dev/null | sort -h | tail -n 60

echo
echo "========== TOP-LEVEL CONTENTS =========="
ls -lah "$ROOT"

echo
echo "========== GIT CHECKOUTS =========="
find "$ROOT" -maxdepth 5 -type d -name .git -print 2>/dev/null

echo
echo "========== LARGE SOURCE ARCHIVES =========="
find "$ROOT" -type f \( -name "*.tar" -o -name "*.tar.xz" -o -name "*.tar.gz" -o -name "*.zip" \) \
  -printf "%s\t%p\n" 2>/dev/null | sort -nr | head -n 20 | numfmt --field=1 --to=iec --suffix=B

echo
echo "========== BUILD DIRECTORY MARKERS =========="
find "$ROOT" -maxdepth 7 -type f \( -name CMakeCache.txt -o -name build.ninja -o -name Makefile \) \
  -printf "%p\n" 2>/dev/null | head -n 50

echo
echo "READ-ONLY: no files modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
