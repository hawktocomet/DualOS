# GBL OS configuration and boot control audit

Report ID: `20261109T004110Z-gbl-os-configuration-and-boot-control-audit`

Exit code: `0`

Command: `bash -c 
echo "===== REPOSITORY REVISION ====="
git -C bootable/libbootloader rev-parse HEAD
echo
echo "===== WORKTREE STATUS ====="
git -C bootable/libbootloader status --short
echo
echo "===== OS CONFIGURATION AND BOOT CONTROL IMPLEMENTATIONS ====="
rg -n -C 12 "GblOsConfigurationProtocol|GblEfiOsConfigurationProtocol|gbl_efi_os_configuration|open_os_configuration|OS_CONFIGURATION|GBL_EFI_OS_BOOT_TARGET_VARNAME|expected_os|TargetOs|enum Os|pub enum Os" bootable/libbootloader/gbl/
echo
echo "===== A/B SLOT CONTROL IMPLEMENTATIONS ====="
rg -n -C 12 "GblBootControlProtocol|GblEfiBootControlProtocol|gbl_efi_boot_control|open_boot_control|current_slot|select_slot|set_active_slot|bootable" bootable/libbootloader/gbl/
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
