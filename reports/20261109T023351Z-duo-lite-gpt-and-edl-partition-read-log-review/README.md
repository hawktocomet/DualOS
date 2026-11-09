# Duo Lite GPT and EDL partition-read log review

Report ID: `20261109T023351Z-duo-lite-gpt-and-edl-partition-read-log-review`

Exit code: `0`

Command: `bash -lc 
ROOT=/home/hawk/duo_forensics/android_boot_chain_20261102_094334

for FILE in gpt_inventory.log edl_partition_read.log; do
  PATH_TO_LOG="$ROOT/$FILE"
  echo
  echo "============================================================"
  echo "FILE: $PATH_TO_LOG"
  echo "============================================================"

  if [ -f "$PATH_TO_LOG" ]; then
    wc -l -c "$PATH_TO_LOG"
    cat "$PATH_TO_LOG"
  else
    echo "MISSING"
  fi
done

echo
echo "LOG REVIEW COMPLETE; NO FILES OR DEVICE PARTITIONS MODIFIED"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
