# pocket-native-tina-toolchain-size-breakdown

Report ID: `20261109T033908Z-pocket-native-tina-toolchain-size-breakdown`

Exit code: `0`

Command: `bash -c 
ROOT="$HOME/source/pocket-recompiler/target/android-clang-builder/tinaide-source/build/tina-toolchain"

echo "========== TOOLCHAIN CONTENTS =========="
du -xhd2 "$ROOT" 2>/dev/null | sort -h | tail -n 50

echo
echo "========== SOURCE DIRECTORY =========="
du -xhd3 "$ROOT/src" 2>/dev/null | sort -h | tail -n 30

echo
echo "========== GENERATED BUILD DIRECTORIES =========="
du -xhd3 "$ROOT/build" 2>/dev/null | sort -h | tail -n 40

echo
echo "READ-ONLY: no files modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
