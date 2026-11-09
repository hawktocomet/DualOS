# DualOS GBL SD-only boot observability source audit

Report ID: `20261109T013802Z-dualos-gbl-sd-only-boot-observability-source-aud`

Exit code: `0`

Command: `bash -c 
cd "$HOME/source/DualOS-tools/aosp-uefi-gbl/bootable/libbootloader" || exit 2

printf "\n=== DIAGNOSTIC FILE ===\n"
if [ -f gbl/efi/src/diagnostic_fs.rs ]; then
  sed -n "1,260p" gbl/efi/src/diagnostic_fs.rs
else
  echo "diagnostic_fs.rs missing"
fi

printf "\n=== EFI LIBRARY DIAGNOSTIC REFERENCES ===\n"
grep -n -C 5 -E "diagnostic_fs|diagnostic|LoadedImage|find_block_devices" gbl/efi/src/lib.rs

printf "\n=== DISK DISCOVERY IMPLEMENTATION ===\n"
grep -n -A 180 -B 20 "fn find_block_devices" gbl/efi/src/efi_blocks.rs

printf "\n=== CONSOLE OUTPUT API ===\n"
sed -n "1,240p" gbl/libefi/src/protocol/simple_text_output.rs

printf "\n=== WORKTREE STATUS ===\n"
git status --short
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
