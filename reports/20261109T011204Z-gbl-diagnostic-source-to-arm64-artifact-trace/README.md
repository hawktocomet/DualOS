# GBL diagnostic source-to-ARM64-artifact trace

Report ID: `20261109T011204Z-gbl-diagnostic-source-to-arm64-artifact-trace`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1

OUT="out/bazel/output_user_root/019cb06479896b5a4be46fc2cc092e63/execroot/_main/bazel-out/k8-fastbuild-ST-1cb3a8a3a143/bin/external/gbl+/efi/aarch64_dev/gbl_aarch64_dev.efi"

echo "===== CONFIGURED TARGET OUTPUT ====="
ls -l "$OUT"
echo "LS_EXIT_CODE=$?"
readlink "$OUT"
echo "READLINK_EXIT_CODE=$?"
readlink -f "$OUT"
echo "RESOLVED_EXIT_CODE=$?"
stat -L -c "mtime=%y size=%s type=%F" "$OUT"
sha256sum "$OUT"
file "$OUT"

echo
echo "===== UNDERLYING ARM64 EFI ARTIFACT ====="
find out/bazel/output_user_root -type f -name "gbl_aarch64_dev.efi" \
  -exec stat -c "%y %s %n" {} \; \
  -exec sha256sum {} \; \
  -exec file {} \;

echo
echo "===== DIAGNOSTIC SOURCE COMPILE ACTION ====="
./bootable/libbootloader/gbl/bazel.sh aquery \
  --extra_toolchains=@gbl//toolchain:all \
  --include_commandline \
  --include_artifacts \
  "inputs(\".*diagnostic_fs.rs\", deps(@gbl//efi:aarch64_dev))" \
  --output=text
echo "DIAGNOSTIC_AQUERY_EXIT_CODE=$?"

echo
echo "===== EFI RUST COMPILE ACTIONS ====="
./bootable/libbootloader/gbl/bazel.sh aquery \
  --extra_toolchains=@gbl//toolchain:all \
  "mnemonic(\"Rustc\", deps(@gbl//efi:aarch64_dev))" \
  --output=summary
echo "RUSTC_AQUERY_EXIT_CODE=$?"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
