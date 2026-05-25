# Suricata Rule Disable Runbook

This runbook covers the process of disabling a specific Suricata IDS rule by SID without removing it from the ruleset. Rules are managed via `suricata-update`; entries added to `disable.conf` persist across all future `suricata-update` runs. The rules file is located at `/var/lib/suricata/rules/suricata.rules` and the disable config at `/etc/suricata/disable.conf`.

---

## Step 1: Find the Signature ID (SID)

Search the rules file for the rule by its message string to identify the SID.

```bash
grep -i "RULE SIGNATURE NAME" /var/lib/suricata/rules/suricata.rules
```

The SID is the numeric value in the `sid:XXXXXXX;` field of the matching rule output.

---

## Step 2: Add the SID to disable.conf

Append the SID to the disable config (one SID per line):

```bash
echo "XXXXXXX" | sudo tee -a /etc/suricata/disable.conf
```

Or edit the file directly:

```bash
sudo nano /etc/suricata/disable.conf
```

> **Note:** Entries in `disable.conf` are permanent and will be applied on every subsequent `suricata-update` run.

---

## Step 3: Reload Rules

Run `suricata-update` to re-merge the ruleset and apply the disable list:

```bash
sudo suricata-update -v
```

---

## Verification

Confirm the following in the command output:

1. **Rule suppressed** — the target SID appears in a `Disabling:` debug line:
   ```
   Disabling: XXXXXXX
   ```

2. **Update completed** — the final line reads:
   ```
   Done.
   ```

3. **Config test passed** — Suricata validated the updated ruleset:
   ```
   Configuration provided was successfully loaded.
   ```

If the SID does not appear under `Disabling:`, verify the entry in `/etc/suricata/disable.conf` contains only the bare numeric SID with no extra whitespace or formatting.
