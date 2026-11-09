# GBL diagnostic EFI artifact resolution

Report ID: `20261109T010239Z-gbl-diagnostic-efi-artifact-resolution`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1

echo "===== BAZEL TARGET OUTPUTS ====="
./bootable/libbootloader/gbl/bazel.sh cquery \
  --extra_toolchains=@gbl//toolchain \
  "@@gbl+//efi:aarch64_dev" \
  --output=files
echo "CQUERY_EXIT_CODE=$?"

echo
echo "===== BAZEL OUTPUT TREE ====="
ls -la bootable/libbootloader/bazel-bin 2>&1
echo
find -L bootable/libbootloader/bazel-bin -maxdepth 7 -type f \
  \( -iname "*.efi" -o -iname "*.efi.signed" -o -iname "*aarch64_dev*" \) \
  -printf "%p %s bytes\n" 2>/dev/null | head -80
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
