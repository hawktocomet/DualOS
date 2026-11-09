# Inventory home directory for cleanup

Report ID: `20261109T030758Z-inventory-home-directory-for-cleanup`

Exit code: `0`

Command: `bash -lc 
HOME_DIR="/home/hawk"

echo "========== FILESYSTEM =========="
df -h "$HOME_DIR"
echo

echo "========== TOP-LEVEL DIRECTORY SIZES =========="
du -x -B1 --max-depth=1 "$HOME_DIR" 2>/dev/null |
  sort -n |
  awk '{ size=$1; $1=""; sub(/^ /,""); printf "%12.2f GiB  %s\n", size/1073741824, $0 }'
echo

echo "========== TOP-LEVEL FILES =========="
find "$HOME_DIR" -maxdepth 1 -type f \
  -printf "%12s bytes  %f\n" | sort -nr
echo

echo "========== HIDDEN TOP-LEVEL DIRECTORIES =========="
find "$HOME_DIR" -mindepth 1 -maxdepth 1 -type d -name ".*" \
  -printf "%f\n" | sort
echo

echo "========== GIT REPOSITORIES =========="
find "$HOME_DIR" -maxdepth 4 -type d -name .git -prune \
  -printf "%h\n" 2>/dev/null | sort
echo

echo "========== VIRTUAL ENVIRONMENTS =========="
find "$HOME_DIR" -maxdepth 4 -type d \
  \( -name .venv -o -name venv -o -name env \) -prune \
  -printf "%p\n" 2>/dev/null | sort
echo

echo "========== LARGEST FILES (DEPTH 4) =========="
find "$HOME_DIR" -maxdepth 4 -type f -printf "%s\t%p\n" 2>/dev/null |
  sort -nr | head -n 60 |
  awk -F "\t" '{ printf "%12.2f MiB  %s\n", $1/1048576, $2 }'
echo

echo "========== BACKUP DESTINATION CHECK =========="
if [ -e "$HOME_DIR/Backups" ]; then
  ls -ld "$HOME_DIR/Backups"
  find "$HOME_DIR/Backups" -mindepth 1 -maxdepth 2 -type d -print | sort
else
  echo "$HOME_DIR/Backups does not exist yet"
fi

echo
echo "INVENTORY ONLY: no files moved, deleted, or modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
