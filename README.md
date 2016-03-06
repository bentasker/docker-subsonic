# Subsonic

[![](https://badge.imagelayers.io/hyzual/subsonic:latest.svg)](https://imagelayers.io/?images=hyzual/subsonic:latest 'Get your own badge on imagelayers.io')

A Dockerfile for the [Subsonic][subsonic] media streamer.

## Subsonic

**Susonic v5.3**.

Subsonic is a personal media streamer. Listen to your music from anywhere – all you need is a browser.

Consider using [Jamstash][jamstash] with your Subsonic container. Jamstash is a HTML5 app for Subsonic, there's also [a container for it][docker-jamstash]!

## Usage

### Using Docker >= 1.9.0

1. Create two named volumes, one for subsonic's configuration (e.g. users) and one for your music.

    This keeps your music and subsonic data persistent and separated from the service so if the container ever goes down and must be recreated the music and data will remain.

    ```shell
    $ sudo docker volume create --name subsonic-data
    $ sudo docker volume create --name subsonic-music
    ```

    Note: you can see the list of all your volumes with this command: `sudo docker volume ls`

2. _(optionnal)_ Import your existing Subsonic configuration and your music into the named volumes.

    Be sure to stop Subsonic first. Replace `/path_to_subsonic_data/` and `/path_to_your_music/` in the commands below.

    ```shell
    $ sudo docker run -it --rm \
        -v subsonic-data:/var/subsonic \
        -v /path_to_subsonic_data/:/hostdata \
        alpine sh
    # Once connected to the new container, copy your data from host to volume
    $ cp -r /hostdata/subsonic/* /var/subsonic
    $ exit
    ```
    To copy your music, the commands are the same, only the volume changes:

    ```shell
    $ sudo docker run -it --rm \
        -v subsonic-music:/var/music \
        -v /path_to_your_music/:/hostmusic \
        alpine sh
    # Once connected to the new container, copy your music from host to volume
    $ cp -r /hostmusic/music/* /var/music
    $ exit
    ```

3. Run the Subsonic container using the named volumes. As long as the volumes are not removed, you can safely remove the Subsonic container, your configuration and music will stay.

    ```shell
        $ sudo docker run -d \
            -p 4040:4040 \
            --name subsonic \
            -v subsonic-data:/var/subsonic \
            -v subsonic-music:/var/music \
            hyzual/subsonic
    ```

### Using Docker < 1.9.0

1. Create 2 containers on your host running docker that will hold your music and data subsonic creates such as users. This keeps your music and subsonic data persistent and separated from the service so if the container ever goes down and must be recreated the music and data will remain. Replace `/path_to_subsonic_data/` and `/path_to_your_music/` below. (You won't have "subsonic data" the first time you setup so just put the path to the folder you want it to be saved in)

```shell
$ docker create --name subsonic-data -v /path_to_subsonic_data/:/var/subsonic busybox
$ docker create --name music-data -v /path_to_your_music/:/var/music busybox
```

2. Run Subsonic using our two data-only containers. As long as they are present, you can remove the Subsonic container, your configuration and music will stay thanks to the data-only containers.

    ```shell
    $ sudo docker run -d \
        -p 4040:4040 \
        --name subsonic \
        --volumes-from subsonic-data \
        --volumes-from music-data \
        hyzual/subsonic
    ```

## Volumes

### /var/subsonic

## Ports

### 4040

Subsonic webserver port

[subsonic]: http://www.subsonic.org
[jamstash]: http://jamstash.com
[docker-jamstash]: https://hub.docker.com/r/hyzual/jamstash/
