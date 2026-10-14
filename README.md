# pokemon
Tool for dealing with VeriFIT's Proxmox environment

## Setup

`pokemon` queries the cluster through [proxxx](https://github.com/fabriziosalmi/proxxx).
Install its binary from the [releases](https://github.com/fabriziosalmi/proxxx/releases)
and create `~/.config/proxxx/config.toml`:

```toml
url = "https://verifit3.fit.vutbr.cz:8006/"
user = "<user>@pve"
auth = "token"
token_id = "<token name>"
token_secret_file = "/home/<you>/.config/proxxx/token"   # absolute path, mode 0600
verify_tls = true
read_only = false   # true blocks `pokemon start/stop`
```

`start` and `stop` need the token to have the `VM.PowerMgmt` privilege, `rm` needs
`VM.Allocate`.

Instead of `token_secret_file`, the secret can also be passed in `$PROXXX_TOKEN_SECRET`.

## Usage

```
pokemon cluster                            # nodes: CPU/memory/disk/local storage usage
pokemon list [-r] [-L]                     # list VMs (-r: only running ones,
                                           # -L: when each VM was last running)
pokemon connect [-l user] [-n] [VM [CMD]]  # ssh to a VM (-n: only print the ssh command)
pokemon start [VM]                         # start a VM
pokemon stop [-f] [-t secs] [--allow-risk] [VM]
                                           # shut a VM down (-f: kill it immediately,
                                           # -t: timeout before killing, default 60)
pokemon rm [--allow-risk] [VM]            # remove a stopped VM incl. its disks
                                           # (asks to type the VM ID to confirm)
pokemon mount [-l user] [-n] [-o opt]... [VM [REMOTE_PATH [MOUNTPOINT]]]
                                           # mount VM's REMOTE_PATH (default: home) via sshfs
                                           # to MOUNTPOINT (default: $HOME/mnt)
```

Without VM, `connect`, `mount` and `stop` let you choose from the running VMs
(`start` and `rm` from the stopped VMs) in a menu
(arrow keys to move, typing filters, Enter selects, Esc cancels; uses
[fzf](https://github.com/junegunn/fzf) if it is installed).

Unmount with `fusermount -u MOUNTPOINT`.

## Shell completion

To complete commands and VM names (e.g., `pokemon connect <Tab>`), add to `~/.zshrc`
(after `compinit`, i.e., after oh-my-zsh is loaded) or `~/.bashrc`:

```
eval "$(pokemon completion zsh)"     # or: eval "$(pokemon completion bash)"
```

The list of VMs is cached for 60 seconds in `~/.cache/pokemon/`.

Environment: `POKEMON_VM_USER` (default user in VMs), `POKEMON_PROXXX`
(path to the proxxx binary), `POKEMON_PROXXX_PROFILE` (proxxx profile).
