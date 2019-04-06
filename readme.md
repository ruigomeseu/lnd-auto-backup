# LND auto backup service

This is a quick and dirty channel backup systemd service for a running LND node. 
It uploads `channel.backup` to a S3 bucket. It uses `inotifywait`
for monitoring for changes and does a new time-stamped backup on each modification.

See [https://github.com/lightningnetwork/lnd/pull/2313](https://github.com/lightningnetwork/lnd/pull/2313) for details.

Tested on my Ubuntu 18.10 server only.

### Prerequisites

* `apt install inotify-tools awscli`

### Setup

1. `git clone --depth=1 https://github.com/darwin/lnd-auto-backup.git` 
2. `cd lnd-auto-backup`
3. create a `.envrc` with content:

```
# note: S3 access must be configured via `aws configure`
export LNDAB_S3_BUCKET=your_bucket_name

# these are optional:
#
#   export LND_HOME=/root/.lnd # if differs from $HOME/.lnd
#   export LND_NETWORK=mainnet
#   export LND_CHAIN=bitcoin
#   export LND_BACKUP_SCRIPT=./backup-via-s3.sh
#   # or if you really need to force it explictly
#   export LNDAB_CHANNEL_BACKUP_PATH=/custom/path/to/channel.backup
```
4. `aws configure` and configure secrets for your AWS S3 account
5. `./service/install.sh`
5. `./service/start.sh` - start it!
6. `./service/status.sh` - just to check the status 
7. `./service/enable.sh` - if it looks good, enable service launching after system restart

#### See logs

`./service/logs.sh`

#### Or just test it

1. source `.envrc` or better use `direnv`
2. `./monitor.sh`