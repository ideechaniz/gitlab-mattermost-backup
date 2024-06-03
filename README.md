# gitlab-mattermost-backup

Gitlab ships mattermost in the omnibus package without backup script.

This repository contains a simple script to backup the mattermost data
## Prerequisites

This fork doesn't use AWS

## Installation

Clone the repository and create a `backup_mattermost.conf` file.

```bash
git clone https://github.com/ideechaniz/gitlab-mattermost-backup.git && cd gitlab-mattermost-backup && cp backup_mattermost.conf.sample backup_mattermost.conf
```

## Configuration

See: [backup_mattermost.conf.sample](backup_mattermost.conf.sample)

## Usage

Just execute the script:

```bash
./backup_mattermost.sh
```

## Setup cron job

Run the backup everyday at 2:00 am everyday.

```bash
crontab -e
```

and add following line:

```bash
0 2 * * * /path/to/backup_mattermost/backup_mattermost.sh
```

## Restore

Install gitlab omnibus and execute following commands:

```bash
tar -zxvf %s_%Y_%m_%d_mattermost.tar.gz -C /tmp/
mv /tmp/mattermost/data/* /var/opt/gitlab/mattermost/
su - gitlab-psql -c "/opt/gitlab/embedded/bin/psql -U gitlab-psql -h /var/opt/gitlab/postgresql -p 5432 mattermost_production" < /tmp/mattermost/mattermost_production_backup.sql
rm -rf /tmp/mattermost
```
