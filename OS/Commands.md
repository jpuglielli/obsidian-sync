**`tee`** is the T-splitter in a pipe — it writes to a file AND passes through to stdout:
```bash
# see output on screen AND save it
kubectl logs my-pod | tee pod-logs.txt

# common pattern: need sudo to write to a file
echo "1" | sudo tee /proc/sys/net/ipv4/ip_forward
```

That `sudo tee` pattern is worth memorizing. `sudo echo "1" > /proc/sys/...` doesn't work because the redirect happens in your shell (not as root). `tee` runs as root via sudo and does the write.
## `grep` — Find Lines Matching a Pattern
Your most-used tool. It filters lines from input.
```bash
# basic: find errors in logs
grep "ERROR" /var/log/app.log

# case-insensitive
grep -i "error" /var/log/app.log

# show N lines of context around matches
grep -B 3 -A 3 "OOMKilled" /var/log/syslog

# recursive through directories
grep -r "DATABASE_URL" /etc/

# invert: show lines that DON'T match
grep -v "healthcheck" access.log

# just count matches
grep -c "500" access.log
```
**The flags that matter:** `-i` (case-insensitive), `-r` (recursive), `-v` (invert), `-c` (count), `-n` (show line numbers), `-A`/`-B`/`-C` (context lines after/before/both).

In your day-to-day: `kubectl logs my-pod | grep -i error` and `grep -r "some_config" /etc/` are reflexes you'll build.
## `find` — Locate Files by Properties
`grep` searches _inside_ files. `find` searches for the files _themselves_.
```bash
# find by name
find /etc -name "*.conf"

# find by modification time (changed in last 24h)
find /var/log -mtime -1

# find large files (over 100MB)
find /var -size +100M

# find and do something (delete old logs)
find /var/log -name "*.gz" -mtime +30 -delete
```

**The syntax pattern is:** `find [where to look] [filters] [action]`

The `-mtime` flag is gold for debugging — "what files changed recently on this system?" is a question you'll ask when something suddenly breaks.
## `xargs` — Turn stdin into Arguments
This bridges `find` (and other tools) to commands that don't read stdin natively.
```bash
# find all .yaml files and grep through them
find . -name "*.yaml" | xargs grep "replicas"

# delete all .pyc files
find . -name "*.pyc" | xargs rm

# handle filenames with spaces safely
find . -name "*.log" -print0 | xargs -0 rm
```
Without `xargs`, the output of `find` is just text on your screen. `xargs` takes that text and feeds it as arguments to another command. The `-0`/`-print0` pair handles filenames with spaces — you'll want this in scripts.
## `awk` — Column Extraction and Text Processing
Think of `awk` as "split each line into columns and let me pick which ones I want." You don't need to learn awk as a full language — just this one pattern covers 90% of usage:
```bash
# default splits on whitespace, $1 is first column, $2 second, etc.
# get PIDs of all python processes
ps aux | grep python | awk '{print $2}'

# get the 5th column (size) from ls
ls -la | awk '{print $5, $9}'

# custom delimiter (like cutting on colons)
cat /etc/passwd | awk -F: '{print $1, $7}'
# → josh /bin/bash

# filter + extract: only lines where column 3 > 80
awk '$3 > 80 {print $1, $3}' scores.txt

# sum a column
awk '{sum += $5} END {print sum}' data.txt
```
**The mental model:** `awk '{action}'` runs that action on every line. `$0` is the whole line, `$1` through `$N` are columns. `-F` sets the delimiter. That's your 80%.
## `sed` — Find and Replace in Streams
`sed` does a lot, but you only need the substitution command:
```bash
# basic find/replace (first match per line)
sed 's/old/new/' file.txt

# global replace (all matches per line)
sed 's/old/new/g' file.txt

# edit file in place
sed -i 's/localhost/0.0.0.0/g' config.yaml

# delete lines matching a pattern
sed '/DEBUG/d' app.log

# replace only on lines matching a pattern
sed '/database/s/5432/5433/g' config.yaml
```
**The pattern:** `s/find/replace/flags` — that's 95% of your `sed` usage. The `-i` flag for in-place editing is what makes it useful in scripts and automation (like patching config files in Dockerfiles).
## `jq` — JSON Swiss Army Knife
Given your Django/API/K8s world, you'll use this constantly since everything speaks JSON.
```bash
# pretty print
curl -s https://api.example.com/data | jq .

# extract a field
kubectl get pod my-pod -o json | jq '.status.phase'

# extract nested fields
jq '.items[].metadata.name' pods.json

# filter array elements
jq '.items[] | select(.status.phase == "Running")' pods.json

# build new objects from existing data
jq '{name: .metadata.name, status: .status.phase}' pod.json
```
**The mental model:** `.` is the current object. `.field` accesses a key. `[]` iterates arrays. `|` pipes within jq (just like shell pipes). `select()` filters.

With `kubectl`, you'll often chain: `kubectl get pods -o json | jq '.items[] | select(.status.phase != "Running") | .metadata.name'` — "show me names of pods that aren't running."