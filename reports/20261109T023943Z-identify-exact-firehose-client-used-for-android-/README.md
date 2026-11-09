# Identify exact Firehose client used for Android partition captures

Report ID: `20261109T023943Z-identify-exact-firehose-client-used-for-android-`

Exit code: `0`

Command: `bash -lc 
echo "========== RECOVERY PROCEDURE READ COMMANDS =========="
for F in \
  /home/hawk/duo_forensics/firehose_method_recovery_20261101_235248.txt \
  /home/hawk/duo_forensics/firehose_method_minimal.txt \
  /home/hawk/duo_forensics/abl_a_firehose_readback.log \
  /home/hawk/duo_forensics/firehose_abl_write_prep_20261101_235214.txt \
  /home/hawk/duo_edl/edl_partition_read.log \
  /home/hawk/duo_edl/edl/README.md
do
  if [ -f "$F" ]; then
    echo
    echo "----- $F -----"
    grep -niE -C 3 "edl\.py|/edl |\.venv/bin/edl|python.*edl|Dumped sector|read.*partition|partition.*read|--loader|--memory" "$F" 2>/dev/null | head -n 180
  fi
done

echo
echo "========== CAPTURE LOG LOCATIONS =========="
find /home/hawk/duo_edl /home/hawk/duo_forensics \
  -maxdepth 7 -type f \
  \( -iname "*partition*read*.log" -o -iname "*edl*.log" -o -iname "*firehose*.log" \) \
  -printf "%TY-%Tm-%Td %TH:%TM %s %p\n" 2>/dev/null | sort

echo
echo "========== CLIENT CHECKOUT IDENTITIES =========="
for D in \
  /home/hawk/duo_edl/edl \
  /home/hawk/duo_forensics/recovery_probe/bkerler-edl
do
  if [ -d "$D/.git" ]; then
    echo "--- $D ---"
    git -C "$D" remote -v 2>/dev/null | head -n 4
    git -C "$D" log -1 --format="%H %cs %s" 2>/dev/null
  fi
done

echo
echo "INSPECTION ONLY: no device commands executed"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
