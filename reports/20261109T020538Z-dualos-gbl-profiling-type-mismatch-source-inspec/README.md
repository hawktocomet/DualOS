# DualOS GBL profiling type mismatch source inspection

Report ID: `20261109T020538Z-dualos-gbl-profiling-type-mismatch-source-inspec`

Exit code: `0`

Command: `bash -c 
printf "\n=== DISK DISCOVERY FUNCTION ===\n"
sed -n "105,225p" gbl/efi/src/efi_blocks.rs

printf "\n=== DEVICE PATH HELPER SIGNATURE ===\n"
grep -R -n -A 12 "fn get_device_path" gbl/efi/src

printf "\n=== DEVICE PATH TEXT TYPE ===\n"
grep -R -n -A 10 "struct DevicePathText\\|enum DevicePathText" gbl
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
