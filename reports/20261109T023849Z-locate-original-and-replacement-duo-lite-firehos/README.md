# Locate original and replacement Duo Lite Firehose tools

Report ID: `20261109T023849Z-locate-original-and-replacement-duo-lite-firehos`

Exit code: `0`

Command: `bash -lc 
echo "========== KNOWN FIREHOSE LOADER =========="
LOADER="/home/hawk/duo_forensics/qcs6125_loader_test/001750e100000000_d40eee56f3194665574109a39267724ae7944134cd53cb767e293d3c40497955bc8a4519ff992b031fadc6355015ac87.bin"
if [ -f "$LOADER" ]; then
  ls -lh "$LOADER"
  sha256sum "$LOADER"
else
  echo "Known loader not found at recorded path"
fi

echo
echo "========== EDL / FIREHOSE EXECUTABLES AND REPOSITORIES =========="
find /home/hawk /opt /usr/local/bin \
  -maxdepth 8 \
  \( -iname "edl.py" -o -iname "edl" -o -iname "qdl" \
     -o -iname "*firehose*" -o -iname "*qualcomm*flash*" \
     -o -iname "*qfil*" \) \
  -print 2>/dev/null | sort -u

echo
echo "========== LIKELY TOOL SOURCE CHECKOUTS =========="
find /home/hawk \
  -maxdepth 7 -type d \
  \( -iname "*edl*" -o -iname "*qdl*" -o -iname "*firehose*" \) \
  -print 2>/dev/null | sort -u

echo
echo "========== SHELL HISTORY REFERENCES =========="
for H in /home/hawk/.bash_history /home/hawk/.zsh_history /home/hawk/.local/share/fish/fish_history; do
  if [ -f "$H" ]; then
    echo "--- $H ---"
    grep -niE "edl|firehose|qdl|qfil|sahara|rawprogram|boot_a|vendor_boot|super" "$H" 2>/dev/null | tail -n 100
  fi
done

echo
echo "========== PREVIOUS CAPTURE SCRIPTS =========="
for F in /home/hawk/duo_edl_report_2.sh; do
  if [ -f "$F" ]; then
    echo "--- $F ---"
    grep -nEi -C 4 "edl|firehose|read|dump|sector|partition|loader" "$F"
  fi
done

echo
echo "SEARCH COMPLETE: no device commands executed"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
