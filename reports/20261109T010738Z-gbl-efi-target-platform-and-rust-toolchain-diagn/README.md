# GBL EFI target platform and Rust toolchain diagnosis

Report ID: `20261109T010738Z-gbl-efi-target-platform-and-rust-toolchain-diagn`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1
G=bootable/libbootloader/gbl

echo "===== MODULE DOCUMENTED BUILD COMMAND ====="
sed -n "1435,1470p" "$G/MODULE.bazel"

echo
echo "===== EFI BUILD TARGET DEFINITIONS ====="
sed -n "90,190p" "$G/efi/BUILD"

echo
echo "===== AARCH64 TOOLCHAIN PLATFORM DEFINITIONS ====="
sed -n "220,325p" "$G/toolchain/BUILD"

echo
echo "===== RUST TOOLCHAIN REGISTRATION ====="
sed -n "680,765p" "$G/toolchain/BUILD"

echo
echo "===== BUILD COMMANDS IN REPOSITORY DOCUMENTATION ====="
rg -n -C 3 "bazel.sh build|extra_toolchains|aarch64_dev|platforms=" "$G" \
  --glob "*.md" --glob "*.sh" --glob "*.py" --glob "MODULE.bazel"

echo
echo "===== BAZEL CONFIGURED TARGET DETAILS ====="
./bootable/libbootloader/gbl/bazel.sh cquery \
  --extra_toolchains=@gbl//toolchain:all \
  @gbl//efi:aarch64_dev \
  --output=build

echo
echo "===== WORKTREE STATUS ====="
git -C "$G" status --short
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
