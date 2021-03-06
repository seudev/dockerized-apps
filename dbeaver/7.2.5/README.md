# Dockerized DBeaver

[![dbeaver](http://dockeri.co/image/seudev/dbeaver)](https://hub.docker.com/r/seudev/dbeaver)

## What is it?

![dockerized-dbeaver-256px](https://raw.githubusercontent.com/seudev/dockerized-apps/master/dbeaver/dockerized-dbeaver-256px.png)

A dockerized [DBeaver CE](https://dbeaver.io/) desktop application.

## Installation

### Docker Pull

```sh
docker pull seudev/dbeaver:7.2.5
```

### Allow access to the X server

*DBeaver* is a GUI application, then to run it in a docker container is necessary allow access to the **X server**. Execute the below script to allow it:

```sh
if ! grep -q "xhost +local:root" ~/.profile; then
    printf "\n# Allow access to the X server, using the local root user\nxhost +local:root\n" >> ~/.profile
    xhost +local:root
fi
```

**Note**: The `xhost +local:root` command can be executed directly, without be append to `~./profile` file, but this mode is necessary re-execute each system boot.

### Create a shortcut on the desktop and in the Start Menu (Optional)

Run the below script to download and configure the shortcut in your desktop and in the Start Menu.

```sh
mkdir -p ~/seudev/dockerized-apps/dbeaver/7.2.5 \
&& wget https://raw.githubusercontent.com/seudev/dockerized-apps/master/dbeaver/dbeaver -O ~/seudev/dockerized-apps/dbeaver/7.2.5/dbeaver \
&& wget https://raw.githubusercontent.com/seudev/dockerized-apps/master/dbeaver/dockerized-dbeaver-256px.png -O ~/seudev/dockerized-apps/dbeaver/dockerized-dbeaver-256px.png \
&& wget https://raw.githubusercontent.com/seudev/dockerized-apps/master/dbeaver/com.seudev.dbeaver.desktop -O ~/Desktop/com.seudev.dbeaver.7.2.5.desktop \
&& sed -i "s/<version>/7.2.5/" ~/seudev/dockerized-apps/dbeaver/7.2.5/dbeaver \
&& sed -i "s/<version>/7.2.5/" ~/Desktop/com.seudev.dbeaver.7.2.5.desktop \
&& sed -i "s/<user>/$USER/" ~/Desktop/com.seudev.dbeaver.7.2.5.desktop \
&& chmod 755 ~/seudev/dockerized-apps/dbeaver/7.2.5/dbeaver ~/Desktop/com.seudev.dbeaver.7.2.5.desktop \
&& cp ~/Desktop/com.seudev.dbeaver.7.2.5.desktop ~/.local/share/applications
```

**Note**: If you want to change the DBeaver version, theme mode or other parameter, then open the `~/seudev/dockerized-apps/dbeaver/7.2.5/dbeaver` file with a text editor and edit it.

## Using Dockerized DBeaver

**Note**: If you has created a shortcut skip this section and just double click `Dockerized DBeaver` shortcut, localizated in your desktop.

```sh
docker run -ti --rm \
    --network host \
    -v ~/dev-workspace:/root/dev-workspace \
    -v ~/seudev/dockerized-apps/dbeaver/volumes/DBeaverData:/root/.local/share/DBeaverData \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix:ro \
    -e GTK_THEME=Adwaita:dark \
    seudev/dbeaver:7.2.5
```

### `docker run` parameters

| **parameter**                                             | **Description**                                                                                                                                |
| --------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `--network host`                                          | Use it if you need to access database server on the host machine                                                                               |
| `-v ~/dev-workspace`                                      | Use the `~/dev-workspace` local directory to share something with the container.                                                               |
| `-v ~/seudev/dockerized-apps/dbeaver/volumes/DBeaverData` | Use the `~/seudev/dockerized-apps/dbeaver/volumes/DBeaverData` local directory to save the dbeaver data like connections, scripts and drivers. |
| `-e DISPLAY=$DISPLAY`                                     | Set the `DISPLAY` environment variable with same local value. It's used by X server.                                                           |
| `-v /tmp/.X11-unix:/tmp/.X11-unix:ro`                     | Bind the `/tmp/.X11-unix` local directory in read only mode. It's used by X server.                                                            |
| `-e GTK_THEME=Adwaita:light`                              | Set the light mode.                                                                                                                            |
| `-e GTK_THEME=Adwaita:dark`                               | Set the dark mode.                                                                                                                             |

### Building this image:

```sh
docker build -t seudev/dbeaver:7.2.5 .
```

## Uninstall

If you want uninstall this dockerized application, then execute the below scripts:

1. Remove the docker image: `docker image rm seudev/dbeaver:7.2.5`
1. Remove the version folder: `rm -rf ~/seudev/dockerized-apps/dbeaver/7.2.5`
1. Remove the shortcut from the Desktop: `rm -f ~/Desktop/com.seudev.dbeaver.7.2.5.desktop`
1. Remove the shortcut from the Start Menu: `rm -f ~/.local/share/applications/com.seudev.dbeaver.7.2.5.desktop`
1. **If you want remove the generated volumes** (connections, scripts and drivers): `sudo rm -rf ~/seudev/dockerized-apps/dbeaver/volumes`
1. **If you want remove all dbeaver folder** (generated volumes, icon and all version folders): `sudo rm -rf ~/seudev/dockerized-apps/dbeaver`

## Licensing

**seudev/dockerized-apps** is provided and distributed under the [Apache Software License 2.0](http://www.apache.org/licenses/LICENSE-2.0).

Refer to *LICENSE* for more information.
