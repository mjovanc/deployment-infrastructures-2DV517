# Setup a new volume with data (one-off task)

**Note:** The data to be copied into the instance should be placed in the folder "volume_files". This folder has a special .gitignore and should **NOT** be included in the repo.

## Run example

*ansible-playbook --extra-vars "volume_name=WP_MYSQL instance_name=WP_MYSQL_1 instance_ip=194.47.176.249" ./utils/volume/add_volume_and_files.yml*

**Note:** 
- The volume_name must match the volume specified in the responsible Wordpress Playbook
- The instance_name and instance_up must exists
- The path to this Playbook (./util/volume...) can depend on local setup 
- The authentication (e.g., clouds file placement) can depend on local setup

## Wordpress (uploads) steps

1. Run *sudo cp -a /home/ubuntu/volume_files/uploads/. /var/www/<WP_HOST>/wordpress/wp-content/uploads/*

The data should be found in **/var/www/<WP_HOST>/wordpress/wp-content/** should now be in the volume.

## MySQL steps

Note: Depending on the playbook the new volume can be mounted to **/var/lib/mysql**. As the files found in this directory should be placed on the volume, the first step is to unmount it as seen in **step 1**

1. *sudo umount /dev/sdb*
2. *sudo mkdir /home/ubuntu/mysql*
3. *sudo cp -a /var/lib/mysql/. /home/ubuntu/mysql*
5. *sudo mount /dev/sdb /var/lib/mysql*
6. *sudo cp -a /home/ubuntu/mysql/. /var/lib/mysql*
7. *sudo service mysql restart*

Volume should now contain the original files.

**Insert the dump data:**

1. Run *mysql -u root -p* (to login as root)
2. Run *use wp;* (to switch to the Wordpress database)
3. Run the SQL file *source /home/ubuntu/volume_files/backup.sql;*

The data should be found in **/var/lib/mysql** and should now be in the volume.
