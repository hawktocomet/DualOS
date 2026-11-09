# home-directory-post-pocket-native-cleanup-audit

Report ID: `20261109T034924Z-home-directory-post-pocket-native-cleanup-audit`

Exit code: `0`

Command: `bash -c 
echo "========== HOME DIRECTORY =========="
printf "Home: %s\n" "$HOME"
echo
echo "========== DISK SPACE =========="
df -h "$HOME"
echo
echo "========== TOP-LEVEL DIRECTORY SIZES =========="
du -x -h --max-depth=1 "$HOME" 2>/dev/null | sort -h
echo
echo "========== LARGEST DIRECTORIES UNDER HOME =========="
du -x -h --max-depth=2 "$HOME" 2>/dev/null | sort -h | tail -60
echo
echo "========== TOP-LEVEL ENTRIES =========="
ls -lah "$HOME"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
