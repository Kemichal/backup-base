## Description
Backup-base contains userful utils for creating and uploading backups. By wrapping up these utils in a container you don't have to install them on the host nor in the containers you want to backup.

## Example usage
The following docker run command executes a backup script inside a backup-base container. The script could for example use the `mysqldump` program to dump a  mysql database, `gzip` the resulting SQL-file and then upload it to Amazon S3 and/or a SMB share. This command could in turn be used in a `crontab`.
```bash
docker run --rm \
  # Mount your backup script in the container
  -v $(pwd)/mysql.sh:/mysql.sh \
  # Set timezone, useful if you want to timestamp the backup
  -v /etc/timezone:/etc/timezone:ro \
  # Join same network as MySQL, could also be --link
  --net my_network \
  # Pass in necessary environment variables
  -e MYSQL_ROOT_PASSWORD \
  # Image name and the script to run
  backup-base /mysql.sh
```
