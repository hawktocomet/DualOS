# DualOS repaired boot flow source validation

Report ID: `20261109T014154Z-dualos-repaired-boot-flow-source-validation`

Exit code: `0`

Command: `bash -c 
printf "\n=== RESTORED ANDROID BOOT FLOW ===\n"
grep -n -E "DUALOS SD-ONLY GBL|find_block_devices|efi_android_load|efi_android_boot|Diagnostic complete|spin_loop" gbl/efi/src/lib.rs

printf "\n=== DISCOVERY REPORTING ===\n"
grep -n -C 3 -E "write_dualos_discovery_report|DUALOS SELECTED|discovery_status|DUALOS FILTER" gbl/efi/src/efi_blocks.rs

printf "\n=== REPORT WRITER ===\n"
grep -n -E "build_report|write_report|DUALOS_DISCOVERY_STATUS" gbl/efi/src/diagnostic_fs.rs

printf "\n=== DIFF CHECK ===\n"
git diff --check
git status --short
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
