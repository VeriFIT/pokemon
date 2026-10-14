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
read_only = true
```

Instead of `token_secret_file`, the secret can also be passed in `$PROXXX_TOKEN_SECRET`.

## Usage

```
pokemon list [-r]                          # list VMs (-r: only running ones)
pokemon connect [-l user] [-n] VM [CMD]    # ssh to a VM (-n: only print the ssh command)
pokemon mount [-l user] [-n] [-o opt]... VM [REMOTE_PATH] [MOUNTPOINT]
                                           # mount VM's REMOTE_PATH (default: home) via sshfs
                                           # to MOUNTPOINT (default: $HOME/mnt)
```

Unmount with `fusermount -u MOUNTPOINT`.

Environment: `POKEMON_VM_USER` (default user in VMs), `POKEMON_PROXXX`
(path to the proxxx binary), `POKEMON_PROXXX_PROFILE` (proxxx profile).
