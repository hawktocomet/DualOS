# DualOS observability patch partial-state audit

Report ID: `20261109T014052Z-dualos-observability-patch-partial-state-audit`

Exit code: `0`

Command: `bash -c 
printf "\n=== ENTRYPOINT IMPORTS AND APP MAIN ===\n"
sed -n "35,190p" gbl/efi/src/lib.rs

printf "\n=== DIAGNOSTIC WRITER SIGNATURES ===\n"
grep -n -E "build_report|write_report|DUALOS_DISCOVERY_STATUS" gbl/efi/src/diagnostic_fs.rs

printf "\n=== DISK DISCOVERY REPORTING ===\n"
grep -n -C 4 -E "write_dualos|DUALOS SELECTED|discovery_status|source_disk_handle" gbl/efi/src/efi_blocks.rs

printf "\n=== DIFF VALIDATION ===\n"
git diff --check
git status --short
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
