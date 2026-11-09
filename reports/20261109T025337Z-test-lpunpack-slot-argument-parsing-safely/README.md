# Test lpunpack slot argument parsing safely

Report ID: `20261109T025337Z-test-lpunpack-slot-argument-parsing-safely`

Exit code: `0`

Command: `bash -lc 
TMP=$(mktemp -d)
echo "========== SLOT OPTION BEFORE IMAGE =========="
lpunpack --slot=0 /nonexistent-super-image "$TMP/a" 2>&1
echo "exit=$?"

echo
echo "========== SLOT OPTION AFTER IMAGE =========="
lpunpack /nonexistent-super-image "$TMP/b" --slot=0 2>&1
echo "exit=$?"

echo
echo "========== SHORT SLOT OPTION =========="
lpunpack -S0 /nonexistent-super-image "$TMP/c" 2>&1
echo "exit=$?"

echo
echo "========== CLEANUP =========="
rmdir "$TMP/a" "$TMP/b" "$TMP/c" "$TMP" 2>/dev/null || true
echo "No source image was opened; no partition extraction occurred"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
