# Setup a new volume with data (one-off task)

**Note:** The data to be copied into the instance should be placed in the folder "volume_files". This folder has a special .gitignore and should **NOT** be included in the repo.

## Run example

*ansible-playbook --extra-vars "volume_name=WORDPRESS_MYSQL instance_name=WORDPRESS_MYSQL_1 instance_ip=194.47.176.249" ./util/volume/add_volume_and_files.yml*

**Note:** 
- The volume_name must match the volume specified in the responsible Wordpress Playbook
- The instance_name and instance_up must exists
- The path to this Playbook (./util/volume...) can depend on local setup 
- The authentication (e.g., clouds file placement) can depend on local setup

[Resource used for formatting and mounting the volume](https://github.com/naturalis/openstack-docs/wiki/Howto:-Creating-and-using-Volumes-on-a-Linux-instance)

## Volume formatting and general steps

A new volume has to be formated for the Linux file system...

**Note:** In the following examples the volume is placed on the /dev/sdb drive, but it can have a different name, e.g., /dev/vdb.

The command *sudo lsblk -f* can be used to check the drive name and verify that the volume is attached.

1. Run *sudo mkfs.ext4 /dev/sdb* to partition the volume 
2. Run *sudo mount /dev/sdb /<PATH>* to mount the volume

The volume should now contain the data found in /<PATH>

## Wordpress (wp-content) steps

1. Run *sudo cp -r /home/ubuntu/volume_files/wp-content/ /var/www/<WP_HOST>/wordpress/wp-content/*
2. Run *sudo mount /dev/sdb /var/www/<WP_HOST>/wordpress/wp-content/*

The data should be found in **/var/www/<WP_HOST>/wordpress/wp-content/** and should now be mounted to the volume.

## MySQL steps

The dump data should be inserted before mounting the volume. This is detailed in the steps below.

1. Run *mysql -u root -p* to login as root
2. Run *use wp;* to switch to the Wordpress database
3. Run the SQL file *source /home/ubuntu/volume_files/backup.sql;*
4. Run *sudo mount /dev/sdb /var/lib/mysql* to mount the volume

The data should be found in **/var/lib/mysql** and should now be mounted to the volume.
