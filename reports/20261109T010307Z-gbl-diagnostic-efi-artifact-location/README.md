# GBL diagnostic EFI artifact location

Report ID: `20261109T010307Z-gbl-diagnostic-efi-artifact-location`

Exit code: `0`

Command: `bash -c 
cd ~/source/DualOS-tools/aosp-uefi-gbl || exit 1

echo "===== BAZEL WRAPPER ====="
sed -n "1,240p" bootable/libbootloader/gbl/bazel.sh

echo
echo "===== EFI FILES IN BUILD TREE ====="
find out/bazel -type f \( -iname "*.efi" -o -iname "*.efi.signed" \) \
  -printf "%p %s bytes\n" 2>/dev/null | tail -80

echo
echo "===== EFI FILES IN REPOSITORY ====="
find bootable/libbootloader -type f \( -iname "*.efi" -o -iname "*.efi.signed" \) \
  -printf "%p %s bytes\n" 2>/dev/null | tail -80

echo
echo "===== BAZEL OUTPUT LINKS ====="
ls -la out/bazel
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
