# GBL diagnostic report writer and device path APIs

Report ID: `20261109T005913Z-gbl-diagnostic-report-writer-and-device-path-api`

Exit code: `0`

Command: `bash -c 
cd bootable/libbootloader || exit 1

echo "===== EXISTING DIAGNOSTIC FILE WRITER ====="
if [ -f gbl/efi/src/diagnostic_fs.rs ]; then
    sed -n "1,280p" gbl/efi/src/diagnostic_fs.rs
else
    echo "MISSING: diagnostic_fs.rs"
fi

echo
echo "===== LOADED IMAGE AND DEVICE PATH HELPERS ====="
sed -n "1,115p" gbl/efi/src/utils.rs
sed -n "1,125p" gbl/libefi/src/protocol/device_path.rs
sed -n "1,100p" gbl/libefi/src/protocol/loaded_image.rs

echo
echo "===== BLOCK IO PROTOCOL AND MEDIA ACCESS ====="
rg -n -B 8 -A 35 "pub struct BlockIoProtocol|impl.*BlockIoProtocol|fn media\\(" gbl/libefi/src

echo
echo "===== DIAGNOSTIC FILESYSTEM REFERENCES ====="
rg -n "write_report|SimpleFileSystem|FileProtocol|open_volume|write\\(" gbl/efi/src gbl/libefi/src
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
