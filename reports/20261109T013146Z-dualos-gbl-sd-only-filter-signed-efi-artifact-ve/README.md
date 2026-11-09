# DualOS GBL SD-only filter signed EFI artifact verification

Report ID: `20261109T013146Z-dualos-gbl-sd-only-filter-signed-efi-artifact-ve`

Exit code: `0`

Command: `bash -c 
ROOT="$HOME/source/DualOS-tools/aosp-uefi-gbl"
OUT="$ROOT/out/bazel/output_user_root/019cb06479896b5a4be46fc2cc092e63/execroot/_main/bazel-out/k8-fastbuild-ST-1cb3a8a3a143/bin/external/gbl+/efi"

printf "%s\n" "=== BUILD OUTPUT TARGET ==="
ls -l "$OUT/aarch64_dev/gbl_aarch64_dev.efi" 2>&1
readlink -f "$OUT/aarch64_dev/gbl_aarch64_dev.efi" 2>&1

printf "%s\n" "=== SIGNED EFI ARTIFACT ==="
ls -l "$OUT/testkey_signed/gbl.efi" 2>&1
file "$OUT/testkey_signed/gbl.efi" 2>&1
sha256sum "$OUT/testkey_signed/gbl.efi" 2>&1

printf "%s\n" "=== UNSIGNED EFI ARTIFACT ==="
ls -l "$OUT/gbl_aarch64_dev.efi" 2>&1
file "$OUT/gbl_aarch64_dev.efi" 2>&1
sha256sum "$OUT/gbl_aarch64_dev.efi" 2>&1

printf "%s\n" "=== BUILD SOURCE REVISION ==="
git -C "$ROOT/bootable/libbootloader" rev-parse HEAD
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
