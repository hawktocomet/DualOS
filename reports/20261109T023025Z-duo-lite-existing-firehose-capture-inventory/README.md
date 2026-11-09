# Duo Lite existing Firehose capture inventory

Report ID: `20261109T023025Z-duo-lite-existing-firehose-capture-inventory`

Exit code: `0`

Command: `bash -lc 
ROOT=/home/hawk/duo_edl/edl

printf "=== CAPTURE ROOT ===\n"
printf "%s\n" "$ROOT"
if [ ! -d "$ROOT" ]; then
  echo "ERROR: capture directory not found"
  exit 2
fi

printf "\n=== DIRECTORY STRUCTURE ===\n"
find "$ROOT" -maxdepth 3 -type d -printf "%p\n" | sort

printf "\n=== CAPTURE FILES (largest first, up to 300) ===\n"
find "$ROOT" -type f -printf "%s\t%p\n" | sort -nr | head -n 300

printf "\n=== PARTITION / FIREHOSE / GPT METADATA CANDIDATES ===\n"
find "$ROOT" -type f \( \
  -iname "*.xml" -o -iname "*.json" -o -iname "*.txt" \
  -o -iname "*.gpt" -o -iname "*partition*" \
  -o -iname "*firehose*" -o -iname "*rawprogram*" \
  -o -iname "*patch*" -o -iname "*prog_*" \
  -o -iname "*.mbn" -o -iname "*.elf" \
\) -printf "%s\t%p\n" | sort -k2

printf "\n=== EXISTING DUMP DIRECTORY ===\n"
if [ -d "$ROOT/new_authorized_dumps" ]; then
  find "$ROOT/new_authorized_dumps" -maxdepth 2 -type f \
    -printf "%s\t%p\n" | sort -k2
else
  echo "new_authorized_dumps not found"
fi

printf "\n=== METADATA SHA256 ===\n"
find "$ROOT" -type f \( -iname "*.xml" -o -iname "*.json" \
  -o -iname "*.txt" -o -iname "*.gpt" \) -print0 |
  sort -z | xargs -0 -r sha256sum

printf "\n=== DEVICE / EDL TOOLING FILES ===\n"
find "$ROOT" -type f \( -iname "*firehose*" -o -iname "*edl*" \
  -o -iname "*qdl*" -o -iname "*loader*" -o -iname "*prog_*" \) \
  -printf "%s\t%p\n" | sort -k2

printf "\n=== INVENTORY COMPLETE; NO DEVICE ACCESS OR WRITES PERFORMED ===\n"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
