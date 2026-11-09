# GBL EFI entrypoint and loaded-image to physical-disk mapping

Report ID: `20261109T004554Z-gbl-efi-entrypoint-and-loaded-image-to-physical-`

Exit code: `0`

Command: `bash -c 
ROOT=bootable/libbootloader/gbl

echo "===== TARGET OS SELECTION ====="
sed -n "65,105p" "$ROOT/efi/src/lib.rs"

echo
echo "===== COMPLETE EFI APP ENTRYPOINT ====="
sed -n "104,330p" "$ROOT/efi/src/lib.rs"

echo
echo "===== LOADED IMAGE PATH HELPER ====="
sed -n "45,80p" "$ROOT/efi/src/utils.rs"

echo
echo "===== LOADED IMAGE PROTOCOL API ====="
rg -n -C 8 \
  "struct LoadedImage|LoadedImageProtocol|fn device_handle|device_handle\(|fn file_path|file_path\(" \
  bootable/libbootloader

echo
echo "===== DISK DISCOVERY CALL SITES ====="
rg -n -C 8 \
  "find_block_devices\(|get_target_os\(|Ops::new\(" \
  "$ROOT/efi/src" --glob "!**/*test*"

echo
echo "===== DEVICE PATH COMPARISON AND PARENT RELATIONSHIP APIs ====="
rg -n -C 6 \
  "LocateDevicePath|locate_device_path|OpenProtocolInformation|open_protocol_information|DevicePathProtocol|DevicePathNode|device_path.*prefix|is_prefix|parent.*handle" \
  bootable/libbootloader

echo
echo "===== DISK HANDLE RETENTION ====="
sed -n "120,205p" "$ROOT/efi/src/efi_blocks.rs"
sed -n "145,215p" "$ROOT/efi/src/ops.rs"

echo
echo "===== END ====="
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
