# pocket-native-generated-android-compiler-build-cleanup

Report ID: `20261109T033956Z-pocket-native-generated-android-compiler-build-c`

Exit code: `0`

Command: `bash -c 
BASE="$HOME/source/pocket-recompiler/target/android-clang-builder/tinaide-source/build/tina-toolchain"
TARGET="$BASE/build/android-aarch64-api28-pathfix0-execwrap0"

echo "========== SAFETY CHECKS =========="

if [ ! -d "$TARGET" ] || [ -L "$TARGET" ]; then
    echo "ABORT: target is missing or is a symlink."
    exit 10
fi

if [ "$(realpath -e "$TARGET")" != "$TARGET" ]; then
    echo "ABORT: target resolves to an unexpected path."
    exit 11
fi

if [ ! -f "$TARGET/CMakeCache.txt" ] || [ ! -f "$TARGET/build.ninja" ]; then
    echo "ABORT: expected generated build markers are missing."
    exit 12
fi

if mountpoint -q "$TARGET"; then
    echo "ABORT: target is a mount point."
    exit 13
fi

echo "Target verified: $TARGET"
du -sh "$TARGET"

echo
echo "========== REMOVE GENERATED BUILD =========="
rm -rf -- "$TARGET"
RESULT=$?

if [ "$RESULT" -ne 0 ] || [ -e "$TARGET" ]; then
    echo "ERROR: removal failed or target still exists."
    exit 20
fi

echo "Removal verified."

echo
echo "========== PRESERVED CONTENTS =========="
du -sh "$BASE/src" "$BASE/.ccache" "$BASE/build/host" 2>/dev/null
du -sh "$HOME/source/pocket-recompiler/target/android-clang-builder/device-test-package"
du -sh "$HOME/source/pocket-recompiler/target/releases"

echo
echo "========== REMAINING TOOLCHAIN BUILD =========="
du -sh "$BASE/build"

echo
echo "CLEANUP COMPLETE"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
