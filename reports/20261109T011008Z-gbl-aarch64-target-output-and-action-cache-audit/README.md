# GBL AArch64 target output and action-cache audit

Report ID: `20261109T011008Z-gbl-aarch64-target-output-and-action-cache-audit`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1
G=bootable/libbootloader/gbl

echo "===== PLATFORM BUILD MACRO ====="
sed -n "235,315p" "$G/toolchain/gbl_toolchain.bzl"

echo
echo "===== EFI PLATFORM TARGET ====="
sed -n "260,295p" "$G/efi/BUILD"

echo
echo "===== CONFIGURED TARGET OUTPUT FILES ====="
./bootable/libbootloader/gbl/bazel.sh cquery \
  --extra_toolchains=@gbl//toolchain:all \
  @gbl//efi:aarch64_dev \
  --output=files
echo "CQUERY_EXIT_CODE=$?"

echo
echo "===== SIGNED TARGET OUTPUT FILES ====="
./bootable/libbootloader/gbl/bazel.sh cquery \
  --extra_toolchains=@gbl//toolchain:all \
  @gbl//efi:gbl_signed \
  --output=files
echo "SIGNED_CQUERY_EXIT_CODE=$?"

echo
echo "===== EFI ACTION GRAPH ====="
./bootable/libbootloader/gbl/bazel.sh aquery \
  --extra_toolchains=@gbl//toolchain:all \
  "deps(@gbl//efi:aarch64_dev)" \
  --output=summary
echo "AQUERY_EXIT_CODE=$?"

echo
echo "===== ARTIFACT IDENTIFICATION ====="
find out/bazel/output_user_root -type f \
  \( -name "gbl_aarch64_dev.efi" -o -path "*/testkey_signed/gbl.efi" \) \
  -exec file {} \; -exec sha256sum {} \; -exec stat -c "%y %s %n" {} \;

echo
echo "===== SOURCE MODIFICATION TIMES ====="
stat -c "%y %n" \
  "$G/efi/src/lib.rs" \
  "$G/efi/src/diagnostic_fs.rs" \
  "$G/efi/src/efi_blocks.rs"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
