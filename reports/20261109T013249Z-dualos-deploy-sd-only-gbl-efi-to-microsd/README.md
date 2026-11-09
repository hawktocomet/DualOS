# DualOS deploy SD-only GBL EFI to microSD

Report ID: `20261109T013249Z-dualos-deploy-sd-only-gbl-efi-to-microsd`

Exit code: `0`

Command: `bash -c 
ROOT="$HOME/source/DualOS-tools/aosp-uefi-gbl"
OUT="$ROOT/out/bazel/output_user_root/019cb06479896b5a4be46fc2cc092e63/execroot/_main/bazel-out/k8-fastbuild-ST-1cb3a8a3a143/bin/external/gbl+/efi"
SRC="$OUT/testkey_signed/gbl.efi"
MNT="/mnt/dualos-check"
EFI="$MNT/EFI/BOOT/BOOTAA64.EFI"
BACKUP="$MNT/EFI/BOOT/BOOTAA64.EFI.pre-sd-only-filter"
EXPECTED_PARTUUID="1b806d74-8676-4e78-a90a-9c209e214d81"
EXPECTED_OLD="367cb02aa0c3bed854926b595ce1bbe5fb63fa2b52b99596b6b9caa93c1ca145"
EXPECTED_NEW="610e939fd609f4bf69c82a71b460aedf95b64c6e676e2f469470ab9c648d4d2e"

printf "%s\n" "=== PRE-FLIGHT ==="
mountpoint -q "$MNT" || { echo "STOP: SD is not mounted at $MNT"; exit 10; }
SOURCE="$(findmnt -n -o SOURCE --target "$MNT")"
PARTUUID="$(sudo blkid -s PARTUUID -o value "$SOURCE" 2>/dev/null)"
FSTYPE="$(findmnt -n -o FSTYPE --target "$MNT")"
printf "Mount source: %s\nPARTUUID: %s\nFilesystem: %s\n" "$SOURCE" "$PARTUUID" "$FSTYPE"

[ "$PARTUUID" = "$EXPECTED_PARTUUID" ] || { echo "STOP: mounted partition is not the verified DualOS SD partition"; exit 11; }
[ "$FSTYPE" = "vfat" ] || { echo "STOP: unexpected filesystem"; exit 12; }
[ -f "$SRC" ] || { echo "STOP: signed build artifact missing"; exit 13; }
[ -f "$EFI" ] || { echo "STOP: target EFI missing"; exit 14; }

ACTUAL_NEW="$(sha256sum "$SRC" | awk "{print \$1}")"
ACTUAL_OLD="$(sudo sha256sum "$EFI" | awk "{print \$1}")"
printf "New artifact: %s\nCurrent SD EFI: %s\n" "$ACTUAL_NEW" "$ACTUAL_OLD"

[ "$ACTUAL_NEW" = "$EXPECTED_NEW" ] || { echo "STOP: build artifact hash mismatch"; exit 15; }
[ "$ACTUAL_OLD" = "$EXPECTED_OLD" ] || { echo "STOP: current SD EFI differs from expected diagnostic build"; exit 16; }

if sudo test -e "$BACKUP"; then
  echo "STOP: backup destination already exists; refusing to overwrite it"
  exit 17
fi

printf "%s\n" "=== PRESERVE CURRENT DIAGNOSTIC EFI ==="
sudo cp -n "$EFI" "$BACKUP" || exit 18
BACKUP_HASH="$(sudo sha256sum "$BACKUP" | awk "{print \$1}")"
[ "$BACKUP_HASH" = "$EXPECTED_OLD" ] || { echo "STOP: diagnostic backup verification failed"; exit 19; }

printf "%s\n" "=== DEPLOY SIGNED SD-ONLY EFI ==="
sudo install -m 0644 "$SRC" "$EFI" || exit 20
DEPLOYED_HASH="$(sudo sha256sum "$EFI" | awk "{print \$1}")"
[ "$DEPLOYED_HASH" = "$EXPECTED_NEW" ] || { echo "STOP: deployed EFI hash mismatch"; exit 21; }

printf "%s\n" "=== FINAL SD STATE ==="
printf "Deployed EFI: "
sudo sha256sum "$EFI"
printf "Preserved diagnostic EFI: "
sudo sha256sum "$BACKUP"
printf "Original EFI backup: "
sudo sha256sum "$MNT/EFI/BOOT/BOOTAA64.EFI.pre-dualos-diagnostic"
echo "DEPLOYMENT_VERIFIED"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
