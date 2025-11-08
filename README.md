# Bash Script Suite — quick manual run guide
This repository contains small, simple Bash scripts for basic system maintenance. They are intentionally short and easy to explain.

Scripts
- `scripts/backup.sh` — create a timestamped tar.gz of paths (default: /etc and /var/log). Use `--dry-run` to avoid writing files.
- `scripts/update.sh` — detect apt/yum/dnf/pacman and run update commands. Use `--simulate` to avoid running real updates.
- `scripts/monitor_logs.sh` — grep logs for patterns and append matches to `logs/alerts.log`. Supports `--follow` for streaming.
- `scripts/maintenance.sh` — small wrapper to run the above scripts with safe flags.

Make executable

```bash
chmod +x scripts/*.sh
chmod +x tests/run_smoke.sh
```

Manual examples (safe by default)

- Backup (dry run):

```bash
bash scripts/backup.sh --dry-run
```

- Backup real (careful, will read system dirs):

```bash
bash scripts/backup.sh /home /etc
```

- Update (simulate):

```bash
bash scripts/update.sh --simulate
```

- Update real (requires sudo/root):

```bash
sudo bash scripts/update.sh
```

- Monitor logs once (default pattern ERROR|WARN|CRITICAL):

```bash
bash scripts/monitor_logs.sh
```

- Monitor logs follow (live streaming into alerts):

```bash
bash scripts/monitor_logs.sh --follow
```

- Run all safe tasks with the wrapper:

```bash
bash scripts/maintenance.sh --all
```

Check logs

All scripts write to `logs/`:

- `logs/backup.log`
- `logs/update.log`
- `logs/alerts.log`

Scheduling (cron) example

Run daily backup at 02:00 (edit with `crontab -e`):

```cron
0 2 * * * /bin/bash /path/to/repo/scripts/backup.sh --dry-run >> /path/to/repo/logs/backup.log 2>&1
```

Run weekly simulated update on Sundays at 03:00:

```cron
0 3 * * 0 /bin/bash /path/to/repo/scripts/update.sh --simulate >> /path/to/repo/logs/update.log 2>&1
```

Notes

- The scripts are intentionally minimal so they can be explained in an interview. They favor clarity over complex features.
- For real production use, add permission checks, better error handling, and secure backup destinations.
