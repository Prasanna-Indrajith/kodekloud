# Create cron job

## Question

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:

a. Install cronie package on all Nautilus app servers and start crond service.

b. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.

## Answer

- Plan - Do all the stuffs through script file
- Create script
```bash
vi script.sh
```

- Inside Script file
```sh
#!/usr/bin/env bash
set -euo pipefail

CRON_EXPR='*/5 * * * * echo hello > /tmp/cron_text'
MARKER='# nautilus-every-5min-echo-hello'

# Install and enable cron service (YUM-based systems)
if ! command -v crond >/dev/null 2>&1; then
  sudo yum install -y cronie
fi
sudo systemctl enable --now crond

# Verify crond is active (non-fatal)
sudo systemctl is-active --quiet crond || { echo "crond not active"; exit 1; }

# Add cron entry for root, avoiding duplicates
# Use crontab -l for root; create a new crontab with the marker and job
tmpfile=$(mktemp)
trap 'rm -f "$tmpfile"' EXIT

# Load existing crontab (if any), remove any previous marker block
sudo crontab -l 2>/dev/null | sed "/$MARKER/,+1d" > "$tmpfile" || true

# Append marker and cron expression
{
  echo "$MARKER"
  echo "$CRON_EXPR"
} >> "$tmpfile"

# Install new crontab for root
sudo crontab "$tmpfile"

echo "Installed cron job for root: $CRON_EXPR"
```

- Modify the script to executable
```bash
sudo chmod +x script.sh
```

- Securely copy the script into 3 App servers
```bash
scp script.sh tony@stapp01
scp script.sh steve@stapp02
scp script.sh banner@stapp03
```

- Log into each server and run the script
```bash
```bash
ssh tony@stapp01
  > Ir0nM@n
./script.sh
```

```bash
ssh steve@stapp02
  > Am3ric@
./script.sh
```

```bash
ssh banner@stapp03
  > Biggr33n
./script.sh
```

