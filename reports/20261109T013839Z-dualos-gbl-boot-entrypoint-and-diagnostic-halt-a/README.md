# DualOS GBL boot entrypoint and diagnostic halt audit

Report ID: `20261109T013839Z-dualos-gbl-boot-entrypoint-and-diagnostic-halt-a`

Exit code: `0`

Command: `bash -c 
printf "\n=== EFI LIBRARY ENTRYPOINT ===\n"
sed -n "1,230p" gbl/efi/src/lib.rs

printf "\n=== LIB.RS LOCAL DIFF ===\n"
git diff -- gbl/efi/src/lib.rs

printf "\n=== DISK FILTER LOCAL DIFF ===\n"
git diff -- gbl/efi/src/efi_blocks.rs

printf "\n=== DISCOVERY CALL SITES ===\n"
grep -R -n -E "find_block_devices\\(" gbl/efi/src
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
