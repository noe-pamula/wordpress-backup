wp-backup
=========

wp-backup is a simple [Docker][1] container that helps you backup and restore your wordpress blog.

 [1]: https://www.docker.com/

## Quick start

Precondition: Given you have a wordpress blog and the corresponding mysql database running in Docker containers. If not, see section "Migrate your blog to Docker", to see how to move your existing blog into a Docker container within minutes.

*Step 1*: Create and run a backup container linked to your wordpress and mysql containers

`docker run --name backup-my-blog --volumes-from=your-wordpress-container --link=your-mysql-container:mysql -d aveltens/wp-backup`

Replace the following values according to your system:

`your-word-press-container`: The name of the Docker container hosting your blog  
`your-mysql-container`: The name of the Docker container hosting your blogs mysql database

*Step 2*: Backup your blog

`docker exec backup-my-blog backup`

Yep. That's all you need to create a complete backup of your blog html pages and database content. The backup is stored in the container, so you won't see any file on your host system for now, but we will come to this later.

*Step 3*: Restore the backup from a specific day

`docker exec backup-my-blog restore 20141114`

Replace 20141114 by the date, you actually made a backup.

All backups are timestamped with the date of the backup. So your blog can move back to any day in history on that you created a backup. The format of the timestamp is `yyyyMMdd` (4 digit year, 2 digit month, 2 digit day). But I am sure you noticed that already.

## Create and run the backup container

The Docker image is available on the public Docker hub under the name aveltens/backup-wordpress.

wp-backup is a separte container, performing backup and restore operations. The wordpress and mysql containers of your blog are linked to wp-backup, but they are not modified in any way.

To run a backup container, you use the `docker run` command, linking your wordpress and mysql containers:

`docker run --name <backup-container-name> --volumes-from=<your-wordpress-container> --link=<your-mysql-container>:mysql -d aveltens/wp-backup`

You have to replace the placeholders:

`<backup-container-name>`: A name of your choice to identify the backup container  
`<your-wordpress-container>`: The name of the wordpress container  
`<your-mysql-container>`: The name of your mysql container  

You may also specify a volume to be able to access the backup files on the Docker host:

`docker run --name <backup-container-name> -v </host/path/to/backups>:/backups --volumes-from=<your-wordpress-container> --link=<your-mysql-container>:mysql -d aveltens/wp-backup`

`</host/path/to/backups>`: an absolute path on the system hosting the containers

After creating a backup you find the backup files on that path on your host system.

## Manual backup

To manually create a backup of your wordpress blog use `docker exec` to run the backup command:

`docker exec <backup-container-name> backup`

`<backup-container-name>`: The name you chose when you created the container with `docker run`.

> Note that `docker exec` requires at leat Docker 1.3.

This will create two archive files under `/backups` in the container. If you mapped a volume you may see those files in the according directory on your host now. They should be named something like `backup_20141030.sql.bz2` and `backup_20141030.tar.gz`.

Explain backup command, timestamp (only one backup per day at the moment) find backup files in the specified volume, Refer to automatic backup

## Restore

> Note that `docker exec` requires at leat Docker 1.3.

## Automatic backups

## Migrate your blog to docker

TODO: Explain how one can backup a blog manually and restore it with wp-backup to a docker container.

## Contact

Please contact me for any questions & feedback: angelo.veltens@online.de
