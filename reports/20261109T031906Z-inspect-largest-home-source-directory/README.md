# inspect-largest-home-source-directory

Report ID: `20261109T031906Z-inspect-largest-home-source-directory`

Exit code: `0`

Command: `bash -lc 
ROOT="/home/hawk/source"

echo "========== SOURCE DIRECTORY =========="
du -sh "$ROOT"
echo

echo "========== IMMEDIATE SUBDIRECTORY SIZES =========="
du -x -B1 --max-depth=2 "$ROOT" 2>/dev/null |
  sort -nr |
  awk "{ printf \"%10.2f GiB  %s\n\", \$1/1073741824, substr(\$0,index(\$0,\$2)) }" |
  head -n 80
echo

echo "========== LARGEST FILES =========="
find "$ROOT" -xdev -type f -printf "%s\t%p\n" 2>/dev/null |
  sort -nr | head -n 40 |
  awk -F "\t" "{ printf \"%10.2f MiB  %s\n\", \$1/1048576, \$2 }"
echo

echo "========== GIT REPOSITORIES =========="
find "$ROOT" -type d -name .git -prune -printf "%h\n" 2>/dev/null
echo

echo "========== GIT WORKTREE CHANGES =========="
find "$ROOT" -type d -name .git -prune -printf "%h\n" 2>/dev/null |
while IFS= read -r repo; do
  echo "--- $repo"
  git -C "$repo" status --short 2>/dev/null | head -n 30
done

echo
echo "READ-ONLY: no files moved, deleted, or modified"
`

Files: `stdout.txt`, `stderr.txt`, `report.json`. Review output for secrets before publishing.
