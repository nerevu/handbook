# Upgrading CKAN to a New Instance

The [Official Instructions](https://helpdesk.links.com.au/kb/faq.php?id=15) are don't have example code, so I wrote this doc. I am not doing the`in-place` instructions.

## Copy needed files from the old instance

- SSH into the old CKAN ec2 instance (if you don't have access, [follow these instructions](https://github.com/nerevu/handbook/blob/master/docs/ec2-instance-connect.md#logging-into-ec2-instance-with-different-keys)).

- Export the `ckan_default` and `datastore_default` PostgreSQL databases using [pg_dump](https://www.postgresqltutorial.com/postgresql-backup-database/).

  When prompted for the password, use the the `postgres` user's password that was previously set (ask Reuben if you don't know it).

  ```bash
  pg_dump -U postgres -W -F t ckan_default > ~/ckan_default_dump.tar

  pg_dump -U postgres -W -F t datastore_default > ~/datastore_default_dump.tar
  ```

- If you have enabled the CKAN filestore, copy it's directory and all files

  1. If you're unsure, check `/etc/ckan/default/production.ini` for the `ckan.storage_path` variable to see if it is set. Copy that directory if it is present

## Restore old instance databases to the new instance

- Once the new instance is up, import both database backups to overwrite the databases of the same name on the new instance.

  - Copy dump files and filestore from the old machine (don't forget to change the IP addresses below)

    ```bash
    # ssh into the new instance
    ssh ec2-user@{new_instance_ip}

    # copy the dump files and filestore to the new instance
    scp ec2-users@{old_instance_ip}:~/ckan_default_dump.tar ~/ckan_default_dump.tar
    scp ec2-users@{old_instance_ip}:~/datastore_default_dump.tar ~/datastore_default_dump.tar
    scp -r ec2-users@{old_instance_ip}:~/ckan ~/
    ```

    *Learn more about the `scp` command [here.](https://medium.com/dev-blogs/transferring-files-between-remote-server-and-local-system-133d78d58137)*

  - Drop and recreate the databases

    ```bash
    # enter postgres terminal
    sudo -u postgres psql
    ```

    ```sql
    -- drop existing connections to ckan_default, drop the table, then recreate it
    SELECT pg_terminate_backend(pg_stat_activity.pid)
    FROM pg_stat_activity
    WHERE pg_stat_activity.datname = 'ckan_default'
      AND pid <> pg_backend_pid();
    DROP DATABASE ckan_default;
    CREATE DATABASE ckan_default;

    -- drop existing connections to datastore_default, drop the table, then recreate it
    SELECT pg_terminate_backend(pg_stat_activity.pid)
    FROM pg_stat_activity
    WHERE pg_stat_activity.datname = 'datastore_default'
      AND pid <> pg_backend_pid();
    DROP DATABASE datastore_default;
    CREATE DATABASE datastore_default;

    -- Hit Ctrl+Z to exit postgres terminal
    ```

    * *`pg_terminate_backend` reference - https://stackoverflow.com/a/5408501*

  - Restore the databases using [pg_restore]()

    ```bash
    # move dump files to root to make them easily findable
    sudo mv ~/ckan_default_dump.tar /
    sudo mv ~/datastore_default_dump.tar /

    # Change permissions to avoid permissions errors
    sudo chmod 777 /ckan_default_dump.tar
    sudo chmod 777 /datastore_default_dump.tar

    # restore the databases using pg_restore as the postgres user
    sudo -u postgres pg_restore --dbname=ckan_default --create --verbose /ckan_default_dump.tar
    sudo -u postgres pg_restore --dbname=datastore_default --create --verbose /datastore_default_dump.tar
    ```

- Extract the filestore archive to the same location on the new instance and set the `ckan.storage_path` value appropriately within the `/etc/ckan/default/production.ini` file (if it is the same name, don't worry about changing the storage_path variable).

  ```bash
  # remove the existing filestore
  sudo rm -rf /var/shared_storage/ckan

  # replace with the old filestore
  sudo mv ~/ckan/ /var/shared_storage/
  ```

- Set the ckan folder's permissions to `apache:apache` (otherwise datasets won't be able to be added)
  ```bash
  sudo -u root chown -R apache:apache /var/shared_storage/ckan/
  ```

- Restart Apache

  ```bash
  sudo systemctl restart httpd
  ```

- Perform a Solr re-index so the new Solr service contains records of the newly imported data. Use the commands below:

  ```bash
  # activate the python virtualenv
  . /usr/lib/ckan/default/bin/activate

  # go to the ckan directory
  cd /usr/lib/ckan/default/src/ckan

  # rebuild the index for the database
  paster --plugin=ckan search-index rebuild --config=/etc/ckan/default/production.ini
  ```

## Check that data was transferred correctly
- [ ] Check that all [ckan users](http://openpeoria.nerevu.com/user/) are still present.
- [ ] Check that all [ckan datasets](http://openpeoria.nerevu.com/dataset) are still present.

## Configure HTTPS on the server
- See [this article](https://handbook.nerevu.com/ckan-https.html).
