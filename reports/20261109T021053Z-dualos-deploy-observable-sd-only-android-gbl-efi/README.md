# DualOS deploy observable SD-only Android GBL EFI

Report ID: `20261109T021053Z-dualos-deploy-observable-sd-only-android-gbl-efi`

Exit code: `0`

Command: `bash -c 
SRC="/home/hawk/source/DualOS-tools/aosp-uefi-gbl/out/bazel/output_user_root/019cb06479896b5a4be46fc2cc092e63/execroot/_main/bazel-out/k8-fastbuild-ST-1cb3a8a3a143/bin/external/gbl+/efi/testkey_signed/gbl.efi"
MNT="/mnt/dualos-check"
PART="/dev/mmcblk0p1"
EFI="/mnt/dualos-check/EFI/BOOT/BOOTAA64.EFI"
BACKUP="/mnt/dualos-check/EFI/BOOT/BOOTAA64.EFI.pre-observable-android-boot"
EXPECTED_PARTUUID="1b806d74-8676-4e78-a90a-9c209e214d81"
EXPECTED_OLD="610e939fd609f4bf69c82a71b460aedf95b64c6e676e2f469470ab9c648d4d2e"
EXPECTED_NEW="4b066ad11e222529a0ad526fa11c20e63ffb1480a819c4a45dbcac71b208ef55"

echo "=== PREFLIGHT ==="
[ -f "$SRC" ] || { echo "STOP: signed artifact missing"; exit 10; }
[ "$(sudo blkid -s PARTUUID -o value "$PART")" = "$EXPECTED_PARTUUID" ] || { echo "STOP: SD PARTUUID mismatch"; exit 11; }
[ "$(findmnt -n -o SOURCE --target "$MNT")" = "$PART" ] || { echo "STOP: wrong mount source"; exit 12; }
[ -f "$EFI" ] || { echo "STOP: active EFI missing"; exit 13; }

NEW_HASH="$(sha256sum "$SRC" | awk "{print \$1}")"
OLD_HASH="$(sudo sha256sum "$EFI" | awk "{print \$1}")"
printf "New artifact: %s\nCurrent EFI: %s\n" "$NEW_HASH" "$OLD_HASH"
[ "$NEW_HASH" = "$EXPECTED_NEW" ] || { echo "STOP: artifact hash mismatch"; exit 14; }
[ "$OLD_HASH" = "$EXPECTED_OLD" ] || { echo "STOP: current EFI hash mismatch"; exit 15; }
sudo test ! -e "$BACKUP" || { echo "STOP: backup path already exists"; exit 16; }

echo "=== REMOUNT READ-WRITE ==="
sudo mount -o remount,rw "$MNT" || { echo "STOP: remount failed"; exit 17; }
findmnt -rn -T "$MNT" -o SOURCE,FSTYPE,OPTIONS,TARGET

echo "=== PRESERVE CURRENT EFI ==="
sudo cp -n "$EFI" "$BACKUP" || { echo "STOP: backup copy failed"; exit 18; }
[ "$(sudo sha256sum "$BACKUP" | awk "{print \$1}")" = "$EXPECTED_OLD" ] || { echo "STOP: backup verification failed"; exit 19; }

echo "=== INSTALL NEW SIGNED EFI ==="
TMP="$MNT/EFI/BOOT/BOOTAA64.EFI.dualos-new"
sudo test ! -e "$TMP" || { echo "STOP: temporary path already exists"; exit 20; }
sudo install -m 0644 "$SRC" "$TMP" || { echo "STOP: install failed"; exit 21; }
[ "$(sudo sha256sum "$TMP" | awk "{print \$1}")" = "$EXPECTED_NEW" ] || { echo "STOP: staged EFI hash mismatch"; exit 22; }
sudo mv -f "$TMP" "$EFI" || { echo "STOP: atomic replacement failed"; exit 23; }
sync

echo "=== FINAL VERIFICATION ==="
DEPLOYED="$(sudo sha256sum "$EFI" | awk "{print \$1}")"
SAVED="$(sudo sha256sum "$BACKUP" | awk "{print \$1}")"
printf "Deployed EFI: %s\nPreserved previous EFI: %s\n" "$DEPLOYED" "$SAVED"
[ "$DEPLOYED" = "$EXPECTED_NEW" ] || { echo "STOP: deployed hash mismatch"; exit 24; }
[ "$SAVED" = "$EXPECTED_OLD" ] || { echo "STOP: preserved backup mismatch"; exit 25; }
echo "DEPLOYMENT_VERIFIED"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
