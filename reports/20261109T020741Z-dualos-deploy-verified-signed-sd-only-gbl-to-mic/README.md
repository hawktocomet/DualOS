# DualOS deploy verified signed SD-only GBL to microSD

Report ID: `20261109T020741Z-dualos-deploy-verified-signed-sd-only-gbl-to-mic`

Exit code: `10`

Command: `bash -c 
set +e

REPO="$HOME/source/DualOS-tools/aosp-uefi-gbl"
SD_PARTUUID="1b806d74-8676-4e78-a90a-9c209e214d81"
MOUNT_DIR="/mnt/dualos-check"

cd "$REPO" || exit 2

printf "\n=== LOCATE SIGNED EFI ARTIFACT ===\n"
mapfile -t ARTIFACTS < <(find out/bazel -type f -path "*/external/gbl+/efi/testkey_signed/gbl.efi" -print 2>/dev/null)

if [ "${#ARTIFACTS[@]}" -ne 1 ]; then
  echo "STOP: expected exactly one signed EFI artifact; found ${#ARTIFACTS[@]}."
  printf "%s\n" "${ARTIFACTS[@]}"
  exit 3
fi

EFI="${ARTIFACTS[0]}"
echo "Artifact: $EFI"
file "$EFI"
EFI_SHA="$(sha256sum "$EFI" | awk "{print \$1}")"
echo "Artifact SHA256: $EFI_SHA"

if [ ! -s "$EFI" ]; then
  echo "STOP: signed EFI artifact is missing or empty."
  exit 4
fi

printf "\n=== IDENTIFY SD PARTITION BY PARTUUID ===\n"
SD_PART="$(sudo blkid -t "PARTUUID=$SD_PARTUUID" -o device 2>/dev/null | head -n 1)"
if [ -z "$SD_PART" ]; then
  echo "STOP: expected SD partition GUID not found."
  exit 5
fi

echo "Matched partition: $SD_PART"
sudo blkid "$SD_PART"
FSTYPE="$(lsblk -no FSTYPE "$SD_PART" | head -n 1)"
if [ "$FSTYPE" != "vfat" ] && [ "$FSTYPE" != "fat" ]; then
  echo "STOP: expected VFAT/FAT filesystem; found: $FSTYPE"
  exit 6
fi

printf "\n=== MOUNT SD PARTITION IF NEEDED ===\n"
CURRENT_MOUNT="$(findmnt -rn -S "$SD_PART" -o TARGET | head -n 1)"
if [ -n "$CURRENT_MOUNT" ]; then
  MOUNT_DIR="$CURRENT_MOUNT"
  echo "Already mounted at: $MOUNT_DIR"
else
  sudo mkdir -p "$MOUNT_DIR" || exit 7
  sudo mount "$SD_PART" "$MOUNT_DIR" || exit 8
  echo "Mounted $SD_PART at $MOUNT_DIR"
fi

MOUNT_SOURCE="$(findmnt -rn -T "$MOUNT_DIR" -o SOURCE | head -n 1)"
echo "Mount source: $MOUNT_SOURCE"
MOUNT_PARTUUID="$(sudo blkid -s PARTUUID -o value "$MOUNT_SOURCE" 2>/dev/null | tr "[:upper:]" "[:lower:]")"
if [ "$MOUNT_PARTUUID" != "$SD_PARTUUID" ]; then
  echo "STOP: mounted filesystem does not match the expected SD partition GUID."
  exit 9
fi

DEST="$MOUNT_DIR/EFI/BOOT/BOOTAA64.EFI"
if [ ! -f "$DEST" ]; then
  echo "STOP: expected existing EFI destination not found: $DEST"
  exit 10
fi

printf "\n=== BACK UP CURRENT SD EFI ===\n"
STAMP="$(date -u +%Y%m%dT%H%M%SZ)"
BACKUP="$DEST.pre-dualos-android-boot-$STAMP"
sudo cp -p "$DEST" "$BACKUP" || exit 11
echo "Backup: $BACKUP"
sudo sha256sum "$BACKUP"

printf "\n=== WRITE NEW SIGNED EFI ===\n"
TMP="$DEST.dualos-new"
sudo install -m 0644 "$EFI" "$TMP" || exit 12
sudo sync
sudo mv -f "$TMP" "$DEST" || exit 13
sudo sync

printf "\n=== VERIFY DEPLOYED EFI ===\n"
DEPLOYED_SHA="$(sudo sha256sum "$DEST" | awk "{print \$1}")"
echo "Build SHA256:     $EFI_SHA"
echo "Deployed SHA256:  $DEPLOYED_SHA"
if [ "$DEPLOYED_SHA" != "$EFI_SHA" ]; then
  echo "DEPLOYMENT FAILED: deployed hash does not match build artifact."
  exit 14
fi

echo "DEPLOYMENT_VERIFIED"
echo "SD partition: $SD_PART"
echo "Mount point: $MOUNT_DIR"
echo "EFI destination: $DEST"
echo "Backup: $BACKUP"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
