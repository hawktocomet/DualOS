# GBL boot and partition discovery audit

Report ID: `20261109T003006Z-gbl-boot-and-partition-discovery-audit`

Exit code: `0`

Command: `bash -c 
echo "===== REPOSITORY REVISION ====="
git -C bootable/libbootloader rev-parse HEAD
echo
echo "===== WORKTREE STATUS ====="
git -C bootable/libbootloader status --short
echo
echo "===== GBL EFI RUST SOURCE FILES ====="
find bootable/libbootloader/gbl/efi/src -maxdepth 2 -type f -name "*.rs" -print | sort
echo
echo "===== ANDROID BOOT / PARTITION / DEVICE DISCOVERY REFERENCES ====="
rg -n -C 8 "impl.*GblOps|GblOps|android_main|get_partition_buffer|partition_size|EFI_BLOCK_IO|LoadedImage|DevicePath|BlockIo|Partition" bootable/libbootloader/gbl/
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
