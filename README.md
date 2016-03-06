# Subsonic

A Dockerfile for the [Subsonic][subsonic] media streamer.

## Subsonic

**Susonic v5.3**.

Subsonic is a personal media streamer. Listen to your music from anywhere – all you need is a browser.

## Usage

1. Create 2 containers on your host running docker that will hold your music and data subsonic creates such as users. This keeps your music and subsonic data persistent and seperated from the service so if the container ever goes down and must be recreated the music and data will remain. Replace `/path_to_subsonic_data/` and `/path_to_your_music/` below. (You won't have "subsonic data" the first time you setup so just put the path to the folder you want it to be saved in)
  ```shell
  $ docker run --name subsonicdata -v /path_to_subsonic_data/:/var/subsonic busybox
  $ docker run --name musicdata -v /path_to_your_music/:/var/music busybox
  ```

2. Run Subsonic using our two data-only containers. As long as they are present, you can remove the Subsonic container, your configuration and music will stay thanks to the data-only containers.

  ```shell
  $ sudo docker run -d -p 4040:4040 --name subsonic --volumes-from subsonicdata --volumes-from musicdata hyzual/subsonic
  ```

## Volumes

### /var/subsonic

## Ports

### 4040

Subsonic webserver port.

[subsonic]: http://www.subsonic.org

