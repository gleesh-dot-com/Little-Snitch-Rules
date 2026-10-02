# little-snitch-rules

Personal [Little Snitch](https://obdev.at/littlesnitch) rule groups, shared between my Macs by subscription.

| Group | File | What's in it |
|---|---|---|
| Browsers | [`rules/browsers.lsrules`](rules/browsers.lsrules) | Safari, Firefox, Brave, Zen, Mail: allow all outgoing |
| Apps | [`rules/apps.lsrules`](rules/apps.lsrules) | Per-app domain rules (Discord, Notion, 1Password, …) |
| Dev Tools | [`rules/dev-tools.lsrules`](rules/dev-tools.lsrules) | iTerm, Zed, Docker, Insomnia, Proxyman, Claude Code |
| Blocks | [`rules/blocks.lsrules`](rules/blocks.lsrules) | Global deny rules for every process |

Processes are matched by code identity (`identifier.TEAMID/bundle.id`), not by path. A rule only
matches an app that's signed by that developer, and it works no matter where the app is installed.

## Subscribing

In Little Snitch: **Rules → Rule Groups → + → Subscribe to Rule Group…** and paste a raw URL:

```
https://raw.githubusercontent.com/<user>/little-snitch-rules/main/rules/browsers.lsrules
https://raw.githubusercontent.com/<user>/little-snitch-rules/main/rules/apps.lsrules
https://raw.githubusercontent.com/<user>/little-snitch-rules/main/rules/dev-tools.lsrules
https://raw.githubusercontent.com/<user>/little-snitch-rules/main/rules/blocks.lsrules
```

- Leave **allow rules enabled** in the subscription dialog. These groups are mostly allow rules.
- The first fetch on a new Mac is made by `Little Snitch Networking`. If it asks, allow it to reach
  `githubusercontent.com`.
- Before subscribing on the Mac the rules came from, delete or disable the local copies so they don't
  duplicate the subscribed ones.
- Blocklists (HaGeZi etc.) are not included here. Subscribe to them separately from Little Snitch's
  featured blocklists.

## Updating

Subscribed rules are read-only in Little Snitch. New rules still get created locally, so every so often:

```bash
sudo "/Applications/Little Snitch.app/Contents/Components/littlesnitch" export-model > model.json
python3 scripts/build_rules.py
git diff rules/
```

`build_rules.py` keeps rules you created and drops factory rules and blocklists. It sorts the rest into
groups using prefix matches in [`groups.json`](groups.json). If a rule matches no group, or looks tied to one
machine (Safari web apps, LAN addresses, build-hash identities, `direction: both`), it goes to
`local/unsorted.lsrules` instead. That folder is gitignored and never published. To publish a new
app's rules, add its identifier to a group in `groups.json`.

`model.json` is a full backup of the Little Snitch config, so it's gitignored too.

## Security

This repo is public: anyone can see which apps these rules cover, and anyone who can push to it can
add allow rules on every subscribed Mac. Keep push access to yourself and turn on 2FA.
