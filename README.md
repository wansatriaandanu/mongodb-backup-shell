# Mongo DB Shell Backup

Shell script for backing up Mongo Databases on a given server.  
You can have your Mongo Database backed up and compressed in a directory and filename of your choice.

## Configuration

At a minimum there are only two lines in the file you will need to edit to make this script work for you.
If you server is not on localhost or requires a username and password you will need to correct that information
at the top of the file.

### Defaults

* HOST: localhost
* PORT: 27017
* USERNAME: blank
* PASSWORD: blank

### Setting Path and File

By default (as the script comes, and if choosen FILE_NAME is empty), the script will generate a file
with the current date as the filename.

When the word 'DATE' appears in the backup path or file name it is replaced by the current date. 
This allows you to put the date the backup was made in the file or a directory itself,
very useful for organizing daily backups when running from a cron job.


* BACKUP_PATH (Line #20): This path is where your backups will be stored (omit trailing slash)
* FILE_NAME (Line #21): This the filename of the tar file that is generated (tar.gz will be appended)

Examples
	
	# create a file called mongo-db.tar.gz in a directory the current date (daily backups)
	BACKUP_PATH="/opt/db-backups/daily/DATE"
	FILE_NAME="mongo-db"

	# create a file with the date in the filename
	BACKUP_PATH="/opt/db-backups"
	FILE_NAME="mongodb.DATE" #ex: mongodb.2011-12-19.tar.gz

### Script Output

On successfully backup the script will inform you of the filesize and filename of the backup

	=> Backing up Mongo Server: localhost:27017
	   connected to: localhost:27017
	=> Success: 4.6M        mongo-db.tar.gz

## Create Cron Job for Scheduller Backup

	sudo crontab -e

It’ll create a new Cron file if you haven’t already created one and open it. Enter this in a new line:

	00 00 * * * /bin/bash /home/username/backup/mongo_backup.sh

## Restoring from Backup

Restore a backup using the `mongorestore` command.

	mongorestore [options] [directory or filename to restore from]

	Examples
	mongorestore /opt/mongo-backups/project_files_production/
	mongorestore /opt/mongo-backups/project_files_production/users.bson

For more information, check out the mongoDB site:

http://www.mongodb.org/display/DOCS/Import+Export+Tools#ImportExportTools-mongodumpandmongorestore

