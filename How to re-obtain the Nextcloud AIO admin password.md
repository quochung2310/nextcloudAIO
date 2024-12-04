# nextcloudAIO
How to re-obtain the Nextcloud AIO admin password #4663

On remote pc, ssh to login onto dietpi (where nextcloud installed), run:

    sudo grep NEXTCLOUD_PASSWORD /var/lib/docker/volumes/nextcloud_aio_mastercontainer/_data/data/configuration.json
