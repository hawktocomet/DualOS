# DualOS diagnostic GBL SD deployment retry

Report ID: `20261109T011432Z-dualos-diagnostic-gbl-sd-deployment-retry`

Exit code: `0`

Command: `bash -c 
CARD=/mnt/dualos-check
TARGET="$CARD/EFI/BOOT/BOOTAA64.EFI"
SOURCE="$PWD/out/bazel/output_user_root/019cb06479896b5a4be46fc2cc092e63/execroot/_main/bazel-out/k8-fastbuild-ST-1cb3a8a3a143/bin/external/gbl+/efi/testkey_signed/gbl.efi"
HOST_BACKUP="$HOME/dualos-sd-backups/20261109T0113Z/BOOTAA64.EFI.original"
CARD_BACKUP="$TARGET.pre-dualos-diagnostic"
EXPECTED="367cb02aa0c3bed854926b595ce1bbe5fb63fa2b52b99596b6b9caa93c1ca145"

echo "===== VERIFY EXISTING HOST BACKUP ====="
if [ ! -f "$HOST_BACKUP" ]; then echo "Missing host backup"; exit 1; fi
sha256sum "$HOST_BACKUP"

echo
echo "===== REMOUNT ====="
sudo mount -o remount,rw "$CARD" || exit 1
findmnt "$CARD"

echo
echo "===== VERIFY CARD AND SOURCE ====="
if [ ! -f "$TARGET" ] || [ ! -f "$SOURCE" ]; then echo "Missing EFI file"; exit 1; fi
if [ -e "$CARD_BACKUP" ]; then echo "Card backup already exists; stopping"; exit 1; fi
if [ "$(sudo sha256sum "$TARGET" | awk "{print \$1}")" != "edc8958b8308cf52d6e6e2d4b3c308c0d8786ee9b27d6b4534e9c72bae9fa4ae" ]; then
  echo "Original card EFI changed; stopping"
  exit 1
fi
if [ "$(sha256sum "$SOURCE" | awk "{print \$1}")" != "$EXPECTED" ]; then echo "Diagnostic source hash mismatch"; exit 1; fi

echo
echo "===== BACK UP ORIGINAL ON CARD ====="
sudo cp -p "$TARGET" "$CARD_BACKUP" || exit 1
sudo sha256sum "$CARD_BACKUP" || exit 1

echo
echo "===== INSTALL DIAGNOSTIC EFI ====="
sudo cp "$SOURCE" "$TARGET.new" || exit 1
if [ "$(sudo sha256sum "$TARGET.new" | awk "{print \$1}")" != "$EXPECTED" ]; then
  echo "Staged file hash mismatch; original remains intact"
  sudo rm -f "$TARGET.new"
  exit 1
fi
sudo mv "$TARGET.new" "$TARGET" || exit 1
sync

echo
echo "===== FINAL VERIFICATION ====="
sudo sha256sum "$TARGET" "$CARD_BACKUP"
sha256sum "$HOST_BACKUP"
sudo file "$TARGET"
if [ "$(sudo sha256sum "$TARGET" | awk "{print \$1}")" = "$EXPECTED" ]; then
  echo "DEPLOYMENT_VERIFIED"
else
  echo "FINAL HASH MISMATCH"
  exit 1
fi
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
