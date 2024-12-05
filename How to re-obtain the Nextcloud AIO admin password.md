# How to properly reset the instance?

If something goes unexpected routes during the initial installation, you might want to reset the AIO installation to be able to start from scratch.

Note

If you already have it running and have data on your instance, you should not follow these instructions as it will delete all data that is coupled to your AIO instance.

Here is how to reset the AIO instance properly:

    Stop all containers if they are running from the AIO interface
    Stop the mastercontainer with sudo docker stop nextcloud-aio-mastercontainer
    If the domaincheck container is still running, stop it with sudo docker stop nextcloud-aio-domaincheck
    Check that no AIO containers are running anymore by running sudo docker ps --format {{.Names}}. If no nextcloud-aio containers are listed, you can proceed with the steps below. If there should be some, you will need to stop them with sudo docker stop <container_name> until no one is listed anymore.
    Check which containers are stopped: sudo docker ps --filter "status=exited"
    Now remove all these stopped containers with sudo docker container prune
    Delete the docker network with sudo docker network rm nextcloud-aio
    Check which volumes are dangling with sudo docker volume ls --filter "dangling=true"
    Now remove all these dangling volumes: sudo docker volume prune --filter all=1 (on Windows you might need to remove some volumes afterwards manually with docker volume rm nextcloud_aio_backupdir, docker volume rm nextcloud_aio_nextcloud_datadir).
    If you've configured NEXTCLOUD_DATADIR to a path on your host instead of the default volume, you need to clean that up as well. (E.g. by simply deleting the directory).
    Make sure that no volumes are remaining with sudo docker volume ls --format {{.Name}}. If no nextcloud-aio volumes are listed, you can proceed with the steps below. If there should be some, you will need to remove them with sudo docker volume rm <volume_name> until no one is listed anymore.
    Optional: You can remove all docker images with sudo docker image prune -a.
    And you are done! Now feel free to start over with the recommended docker run command!

# Question about changing data DIR? #890

https://github.com/nextcloud/all-in-one/discussions/890

Stopping all containers from the aio interface,

Creating a backup,

Use rsync to sync the files from the old datadir (in your case probably /var/lib/docker/volumes/nextcloud_aio_nextcloud_data/_data/ into e.g. /mnt/ncdata) and then removing and recreating the mastercontainer with -e NEXTCLOUD_DATADIR="/mnt/ncdata" and also starting the containers again from the aio interface.


# How to re-obtain the Nextcloud AIO admin password #4663

On remote pc, ssh to login onto dietpi (where nextcloud installed), run:

    sudo grep NEXTCLOUD_PASSWORD /var/lib/docker/volumes/nextcloud_aio_mastercontainer/_data/data/configuration.json
# How to connect NextCloud to Gmail (for admin)
The server address is “smtp.gmail.com” port 465.

Create an app password on your gmail account

Use your gmail username without @ and the app password to authenticate.
