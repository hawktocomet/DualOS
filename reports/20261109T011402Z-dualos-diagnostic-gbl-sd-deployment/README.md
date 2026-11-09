# DualOS diagnostic GBL SD deployment

Report ID: `20261109T011402Z-dualos-diagnostic-gbl-sd-deployment`

Exit code: `1`

Command: `bash -c 
CARD=/mnt/dualos-check
DEVICE=/dev/mmcblk0p1
TARGET="$CARD/EFI/BOOT/BOOTAA64.EFI"
SOURCE="out/bazel/output_user_root/019cb06479896b5a4be46fc2cc092e63/execroot/_main/bazel-out/k8-fastbuild-ST-1cb3a8a3a143/bin/external/gbl+/efi/testkey_signed/gbl.efi"
EXPECTED="367cb02aa0c3bed854926b595ce1bbe5fb63fa2b52b99596b6b9caa93c1ca145"
BACKUP_DIR="$HOME/dualos-sd-backups/20261109T0113Z"
BACKUP="$BACKUP_DIR/BOOTAA64.EFI.original"
CARD_BACKUP="$TARGET.pre-dualos-diagnostic"

echo "===== PRE-FLIGHT ====="
if [ "$(findmnt -n -o SOURCE --target "$CARD")" != "$DEVICE" ]; then
  echo "ERROR: Unexpected mounted device"
  exit 1
fi
if [ ! -f "$TARGET" ] || [ ! -f "$SOURCE" ]; then
  echo "ERROR: Source or target file missing"
  exit 1
fi
if [ "$(sha256sum "$SOURCE" | awk "{print \$1}")" != "$EXPECTED" ]; then
  echo "ERROR: Diagnostic EFI hash mismatch"
  exit 1
fi
if [ -e "$BACKUP" ] || [ -e "$CARD_BACKUP" ]; then
  echo "ERROR: Backup already exists; refusing to overwrite"
  exit 1
fi

echo "Card: $(findmnt -n -o SOURCE,FSTYPE --target "$CARD")"
echo "Original EFI:"
sha256sum "$TARGET"
echo "Diagnostic EFI:"
sha256sum "$SOURCE"

echo
echo "===== CREATE HOST BACKUP ====="
mkdir -p "$BACKUP_DIR" || exit 1
cp -p "$TARGET" "$BACKUP" || exit 1
sha256sum "$BACKUP"

echo
echo "===== REMOUNT CARD READ-WRITE ====="
mount -o remount,rw "$CARD" || exit 1
findmnt "$CARD"
case ",$(findmnt -n -o OPTIONS --target "$CARD")," in
  *,rw,*) ;;
  *) echo "ERROR: Card is not mounted read-write"; exit 1 ;;
esac

echo
echo "===== BACK UP ORIGINAL ON CARD ====="
cp -p "$TARGET" "$CARD_BACKUP" || exit 1
sha256sum "$CARD_BACKUP"

echo
echo "===== INSTALL DIAGNOSTIC EFI ====="
cp "$SOURCE" "$TARGET.new" || exit 1
if [ "$(sha256sum "$TARGET.new" | awk "{print \$1}")" != "$EXPECTED" ]; then
  echo "ERROR: Staged file hash mismatch; original untouched"
  rm -f "$TARGET.new"
  exit 1
fi
mv "$TARGET.new" "$TARGET" || exit 1
sync

echo
echo "===== FINAL VERIFICATION ====="
sha256sum "$TARGET" "$CARD_BACKUP" "$BACKUP"
file "$TARGET"
stat -c "%s bytes %n" "$TARGET"

if [ "$(sha256sum "$TARGET" | awk "{print \$1}")" = "$EXPECTED" ]; then
  echo "DEPLOYMENT_VERIFIED"
else
  echo "ERROR: Final EFI hash mismatch"
  exit 1
fi
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
