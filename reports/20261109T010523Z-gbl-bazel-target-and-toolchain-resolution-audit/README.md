# GBL Bazel target and toolchain resolution audit

Report ID: `20261109T010523Z-gbl-bazel-target-and-toolchain-resolution-audit`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1

echo "===== TOOLCHAIN BUILD FILE ====="
sed -n "1,280p" bootable/libbootloader/gbl/toolchain/BUILD

echo
echo "===== TOOLCHAIN TARGET DEFINITIONS ====="
rg -n "aarch64_dev|toolchain_type|toolchain\\(|register_toolchains|extra_toolchains" \
  bootable/libbootloader/gbl/toolchain \
  bootable/libbootloader/gbl/WORKSPACE \
  bootable/libbootloader/gbl/MODULE.bazel \
  bootable/libbootloader/gbl/BUILD 2>/dev/null

echo
echo "===== BAZEL TARGET QUERY: EFI PACKAGE ====="
./bootable/libbootloader/gbl/bazel.sh query "@gbl//efi:*"
echo "EFI_QUERY_EXIT_CODE=$?"

echo
echo "===== BAZEL TARGET QUERY: TOOLCHAIN PACKAGE ====="
./bootable/libbootloader/gbl/bazel.sh query "@gbl//toolchain:*"
echo "TOOLCHAIN_QUERY_EXIT_CODE=$?"

echo
echo "===== BAZEL CONFIGURATION ====="
find bootable/libbootloader/gbl -maxdepth 2 \
  \( -name ".bazelrc" -o -name "MODULE.bazel" -o -name "WORKSPACE" \) \
  -type f -print

echo
echo "===== WORKSPACE REVISION AND MODIFICATIONS ====="
git -C bootable/libbootloader rev-parse HEAD
git -C bootable/libbootloader status --short
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
