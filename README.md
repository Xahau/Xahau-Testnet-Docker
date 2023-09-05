# Xahaud Docker Container

Docker container to run a Xahau node on Testnet (Hooks V3 testnet) with Proof of Burn xPOP import.

## XAHAU TESTNET, NETWORK ID 21338

```
                   $$\                                 $$\ 
                   $$ |                                $$ |
$$\   $$\ $$$$$$\  $$$$$$$\   $$$$$$\  $$\   $$\  $$$$$$$ |
\$$\ $$  |\____$$\ $$  __$$\  \____$$\ $$ |  $$ |$$  __$$ |
 \$$$$  / $$$$$$$ |$$ |  $$ | $$$$$$$ |$$ |  $$ |$$ /  $$ |
 $$  $$< $$  __$$ |$$ |  $$ |$$  __$$ |$$ |  $$ |$$ |  $$ |
$$  /\$$\\$$$$$$$ |$$ |  $$ |\$$$$$$$ |\$$$$$$  |\$$$$$$$ |
\__/  \__|\_______|\__|  \__| \_______| \______/  \_______|

>>> TESTNET EDITION
>>> HOOKS V3 TESTNET WITH PROOF OF BURN IMPORT

>>> NETWORK ID: [ 21338 ]
```

### Buid:

WARNING!
- This will remove any existing image with the same `xahaud` release tag
- The container tag will `xahaud-testnet`

```bash
./build
```

### Run:

WARNING! This will remove any running container called `xahaud-testnet`!

```bash
./up
```

### Config & database

Config and database perist in `./store/etc` and `./store/db`. Logfiles in `./store/log`.

Feel free to clean the `db` and `log` folder at your convenience, contents will be re-created.

### Xahaud server identity

The `xahaud` server identity will perist as long as you keep the contents of `wallet.db`
in the `./store/db` folder.

You can stop, remove & re-create the container: as long as this file persists your server
identity will be the same after a restart.

### Remote access

This container runs `ssh` as well. You can SSH into the container, but for every build the
SSH server identity will change. The user is `root`, so: `ssh root@localhost -p 2222`.

To allow SSH access (key only), add your pubkey(s) to `./store/ssh/authorized_keys`

NOTE: not a hidden file, don't start with a dot). Make sure to `chmod 400` the file.
Make sure the file is owned by `root` (chown root:root).

### Commands

To get the latest server status (or run another `xahaud` command):

```bash
# docker exec {container} {binary} {cliopts}
docker exec xahaud-testnet xahaud server_info
```

To check the current sync status:

```bash
# docker exec {container} {binary} {cliargs} {cliopts} | grep {string to match}
docker exec xahaud-testnet xahaud -q server_info | grep complete_ledgers
```

To check the peer connections:

```bash
# docker exec {container} {binary} {cliargs} {cliopts} | grep {string to match}
docker exec xahaud-testnet xahaud -q peers|grep address
```

To check the live Xahaud logs:

```bash
# docker exec {container} {binary} {cliargs} {cliopts} | grep {string to match}
docker exec xahaud-testnet tail -f /opt/xahaud/log/debug.log
```

To monitor sync status (exit with `CTRL-C`):
```bash
watch 'docker exec xahaud-testnet xahaud -q server_info | grep complete_ledgers'
```

To start/stop/restart `xahaud`:

```bash
systemctl start xahaud
systemctl restart xahaud
systemctl stop xahaud
```

To prevent `xahaud` from auto-starting when the container starts:

```bash
systemctl disable xahaud
```

---

Enjoy!
