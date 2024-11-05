# Gentoo Update Script

**Repository**: [gentoo_update](https://github.com/wbtek/gentoo_update)  
**Author**: WBTek, a division of WhiteBear Family Inc.  
**Description**: This script automates system updates and essential post-update actions for Gentoo Linux systems.

## Overview

`gentoo_update` is a Bash script designed to streamline the process of updating a Gentoo Linux system and performing recommended follow-up actions. Developed by WBTek, a company with over 25 years of experience in embedded control and custom technical solutions, this script is ideal for system administrators and advanced Gentoo users who need a reliable, automated update solution.

## Usage

```bash
./gentoo_update [modifiers] [first_action]
```

The script performs various update actions for Gentoo, based on user-specified commands and modifiers. For a detailed list of options, run:

```bash
./gentoo_update help
```

### Important Note

- **Root Permissions**: `gentoo_update` requires root or sudo privileges to execute system updates. If run without sufficient privileges, it will automatically enter "pretend" mode, allowing users to see the intended actions without making actual changes to the system.

## Help Output

Below is the verbatim output from running `gentoo_update help`:

```plaintext
./gentoo_update: Update Gentoo. Do <first_action> and those to right below.

Usage: gentoo_update [help] [modifiers] [<first_action>]
  modifiers: only pretend quick strict exhaustive debug
  first_action: (sync|portage|emerge|depclean|preserved|revdep[do])

Actions: (do <first_action> to end unless [only] specified)
  'sync' -- sync local portage database from web (1x/day max please!).
  'portage' -- fetch and update packages of portage.
  'emerge' -- fetch and update everything installed using portage.
  'depclean' -- removes unused packages after user confirmation.
  'preserved' -- rebuild to allow removal of old library versions.
  'revdep' -- check whether libraries changed under packages.

Special actions:
  'system' -- 'emerge' with @system, not @world. Flows to 'depclean'.
  'revdepdo' -- same as 'revdep' but rebuilds compromised packages.

Modifiers:
  'only' stops after only one action (doesn't do rest of list).
  'pretend' shows commands without executing them.
  'quick' doesn't add several deepish options to 'emerge'.
  'strict' almost 'exhaustive' but builds less.
  'exhaustive' makes 'emerge' build if dependencies changed.
  'debug' shows diagnostics. Shows more as first option.

Examples:
  'gentoo_update sync' does *all* actions in list above.
  'gentoo_update emerge' does very deep 'emerge' and all remaining steps.
  'gentoo_update only quick system' stops after shallow emerge of @system'.

Default is 'only emerge' when no <first_action> given, so:
  'gentoo_update' is shortcut for 'gentoo_update only emerge'
    (stops after very deep emerge)
  'gentoo_update quick' is shortcut for 'gentoo_update quick only emerge'.
    (stops after shallow emerge)
```

## Note

  All actions below specified `first_action` in the `Actions` list above are run, so
  ```bash
  gentoo_update depclean
  ```
  will also attempt `preserved` and `revdep`. `sync` does all actions, being first on the list.

## Examples

- **Full Update with sync and deepest dependency checks**:  
  ```bash
  gentoo_update sync
  ```
  is the same as
  ```bash
  gentoo_update exhaustive sync
  ```
  Performs sync and exhaustive emerge with all follow-up actions.

- **Deepest Emerge with All Steps but sync**:  
  ```bash
  gentoo_update strict emerge
  ```
  Executes a less deep `emerge` and all subsequent actions.

- **Deepest Emerge then stop (default)**:  
  ```bash
  gentoo_update
  ```
  is the same as
  ```bash
  gentoo_update only exhaustive emerge
  ```
  Executes an exhaustive `emerge`, then stops.

- **Only Quick Emerge**:  
  ```bash
  ./gentoo_update quick
  ```
  is the same as
  ```bash
  ./gentoo_update only quick emerge
  ```
  Limits the script to a shallow `emerge`, then stops.

## License

MIT License. See `LICENSE` file for more details.
