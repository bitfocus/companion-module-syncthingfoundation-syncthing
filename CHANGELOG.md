# Changelog

All notable changes to this module are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow
[Semantic Versioning](https://semver.org/).

## [Unreleased]

Nothing yet.

## [1.0.0] - 2026-10-02

First release.

### Connection and setup

- Connects to the REST API of a single Syncthing instance over HTTP or HTTPS, with an option to
  accept the self-signed certificate Syncthing generates for its own web interface.
- Finds Syncthing instances on the local network by listening for the announcements they
  broadcast, checks that their web interface actually answers, and offers them for selection with
  their machine name and short device ID. Any other address can be typed in.
- Can read the API key from an instance whose web interface has no password, and stores it as a
  Companion secret. A protected instance is reported in the log, and the key is then entered by
  hand.
- A new connection contacts nothing until a host has been chosen.

### Staying up to date

- Follows the Syncthing event stream, so folder state, transfer progress, pause and resume, and
  devices coming and going reach buttons within milliseconds.
- Polls underneath as a safety net, so a missed event cannot leave the state wrong, and reads the
  expensive folder status only rarely while events are flowing.
- Recovers from a broken connection and from a restarted Syncthing by itself.
- Follows changes to the instance's folders and devices without reloading the connection.

### Actions

- Rescan all folders, or a single folder.
- Pause, resume or toggle a folder or a device, or pause and resume every device at once.
- Override remote changes on a send-only folder, and revert local changes on a receive-only folder.
- Restart Syncthing, shut it down, clear its error list, and refresh the status immediately.

### Feedbacks

- Connected to Syncthing, restart required, and Syncthing reports errors.
- This machine is up to date, and in sync with all other devices, covering both directions.
- Any folder is syncing. Per folder: state, fully in sync, paused, and failed files. Per device:
  connected, paused, and fully in sync.

### Variables

- Instance-wide: connection, version, platform, own device ID and name, the address of the web
  interface, uptime, byte totals, counts of folders and devices, overall completion, sync state,
  errors, a pending restart, and what the network search has found.
- Per folder and per device, each under two names: one derived from the identifier, which survives
  renaming, and one derived from the label or device name, which reads better.

### Presets

- An overview of sync state, connection, devices, transfers and errors.
- One button per folder and per device, generated from the instance's configuration, with pause
  buttons and, where they apply, override and revert.
- Control buttons for rescanning, clearing errors, refreshing, pausing all devices and restarting.
