# Gentoo Update Script

**Repository**: [gentoo_update](https://github.com/wbtek/gentoo_update)  
**Author**: WBTek, a division of WhiteBear Family Inc.  
**Description**: This script automates system updates and essential post-update actions for Gentoo Linux systems.

## Overview

`gentoo_update` is a bash script to take care of the follow-up actions needed while updating a Gentoo Linux system. Specify where to start and it does the rest. For instance, if you have already done a 'sync' (which should only be done once a day), you can start at 'emerge' and it will update everything, clean unused dependencies, check for preserved libraries then do reverse dependency checks. Or you can start at 'sync' and it will first try to update portage itself, then do emerge and all the steps following that. By default it does 'strict' dependency checking. You can also specify 'quick' or '(ex)haustive' to modify that. You can also do steps individually.

## Usage

```bash
gentoo_update [modifiers] [first_action]
```

Performs sequences of actions required to maintain Gentoo, with a single command. For a detailed list of options, run:

```bash
gentoo_update help
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

Actions: (do <first_action> and all following unless [only] specified)
  'sync' -- sync local portage database from web (1x/day max please!).
  'portage' -- fetch and update portage itself. (hopefully redundant)
  'system' -- fetch and update everything @system. (seldom needed)
  'emerge' -- fetch and update everything @world. (usually enough)
  'depclean' -- removes unused packages after user confirmation.
  'preserved' -- rebuild to allow removal of old library versions.
  'revdep' -- check whether libraries changed under packages.

Special actions:
  'system' -- 'emerge' with @system, not @world. Flows to 'depclean'.
  'revdepdo' -- same as 'revdep' but rebuilds compromised packages.

Modifiers (abreviated form in parenthesis):
  'only' stops after only one action (doesn't do rest of list).
  'pretend' shows commands without executing them.
  'quick' minimal dependency checking.
  'strict' (default) 'quick' + quite good dependency checking.
  '(ex)haustive' 'strict' + rebuilds if any dependencies rebuilt.
  'debug' shows diagnostics. Shows more as first option.

Examples:
  'gentoo_update sync' does ALL actions in list above.
  'gentoo_update emerge' sets 'strict' mode and starts from 'emerge'.
  'gentoo_update only quick system' stops after quick emerge of @system'.

Default is 'strict only emerge' when no <first_action> given, so:
  'gentoo_update' is shortcut for 'gentoo_update strict only emerge'
    (stops after strict emerge)
  'gentoo_update quick' is short for 'gentoo_update quick only emerge'.
    (stops after quick emerge)
```

## Note

  All actions below specified `first_action` in the `Actions` list above are run, so
  ```bash
  gentoo_update depclean
  ```
  will also attempt `preserved` and `revdep`. `sync` as `first_action` would do all actions, as it is first in the list.

## Examples

- **Sync, emerge and all follow-up actions (should only sync once a day)**:
  ```bash
  gentoo_update sync
  ```
- **Emerge and all follow-up actions but no sync (should only sync once a day)**:
  ```bash
  gentoo_update emerge
  ```
- **Emerge, then stop (no other actions)**:
  ```bash
  gentoo_update
  ```
- **Emerge with exhaustive dependency checking, then stop (no other actions)**:
  ```bash
  gentoo_update ex (or exhaustive)
  ```
- **Quick emerge with relaxed dependency checking, then stop (no other actions)**:
  ```bash
  gentoo_update quick
  ```
- **Build packages found by reverse dependency checking (no other actions)**:
  ```bash
  gentoo_update revdepdo
  ```
- **See what command would do without doing anything**:
  ```bash
  gentoo_update pretend <other modifiers> <first action>
  ```

## License

MIT License. See `LICENSE` file for more details.
