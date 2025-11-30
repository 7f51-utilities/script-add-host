# script-add-host

A small Python utility to add host entries to a hosts file in a safe, idempotent way.

## What it does

- Reads the system hosts file (or a custom path) and looks for a managed block identified by a header/footer comment.
- Adds a host entry (IP + hostname) inside that block if it does not already exist.
- Creates the managed block if it's missing.

## Requirements

- Python 3.10 or newer (the code uses modern type hints like `list[str] | None`).

## Usage

Open a terminal and run:

```powershell
python add-host.py <hostname> [--host_address <ip>] [--custom_path <path_to_hosts>]
```

Examples:

- Add `example.test` mapped to `127.0.0.1` (default address):

```powershell
python add-host.py example.test
```

- Add `myapp.local` with a custom IP:

```powershell
python add-host.py myapp.local --host_address 192.168.1.42
```

- Operate on a custom hosts file (for testing):

```powershell
python add-host.py example.test --custom_path C:\path\to\hosts_test
```

## Permissions

- On Windows you must run the script as an Administrator to modify `C:\Windows\System32\drivers\etc\hosts`.
- On macOS/Linux run with `sudo` to modify `/etc/hosts`.

## Environment

- The script uses the environment variable `APP_NAME` (default: `web-server`) to build the header/footer comments it places into the hosts file. Example header/comment lines look like:

```
# START Added by web-server hosts
# END web-server hosts
```

Set `APP_NAME` to change the marker text:

```powershell
$env:APP_NAME = 'my-app'
python add-host.py example.test
```

## Safety notes

- The script will not duplicate entries inside the managed block; it checks for an existing identical entry before writing.
- Always review any changes when running the script against the system hosts file.

## Development

- Source: `add-host.py` (single-file script)
