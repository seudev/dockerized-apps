# Dockerized Jaspersoft Studio

[![jaspersoft-studio](http://dockeri.co/image/seudev/jaspersoft-studio)](https://hub.docker.com/r/seudev/jaspersoft-studio)

## What is it?

![dockerized-jaspersoft-studio-256px](https://raw.githubusercontent.com/seudev/dockerized-apps/master/jaspersoft-studio/dockerized-jaspersoft-studio-256px.png)

A dockerized [Jaspersoft Studio CE](https://community.jaspersoft.com/project/jaspersoft-studio) desktop application.

## Installation

### Docker Pull

```sh
docker pull seudev/jaspersoft-studio:5.5.1
```

### Allow access to the X server

*Jaspersoft Studio* is a GUI application, then to run it in a docker container is necessary allow access to the **X server**. Execute the below script to allow it:

```sh
if ! grep -q "xhost +local:root" ~/.profile; then
    printf "\n# Allow access to the X server, using the local root user\nxhost +local:root\n" >> ~/.profile
    xhost +local:root
fi
```

**Note**: The `xhost +local:root` command can be executed directly, without be append to `~./profile` file, but this mode is necessary re-execute each system boot.

### Create a shortcut on the desktop (Optional)

Run the below script to download and configure the shortcut in your desktop.

```sh
mkdir -p ~/.jaspersoft-studio \
&& wget https://raw.githubusercontent.com/seudev/dockerized-apps/master/jaspersoft-studio/jaspersoft-studio -O ~/.jaspersoft-studio/jaspersoft-studio \
&& wget https://raw.githubusercontent.com/seudev/dockerized-apps/master/jaspersoft-studio/dockerized-jaspersoft-studio-256px.png -O ~/.jaspersoft-studio/dockerized-jaspersoft-studio-256px.png \
&& wget https://raw.githubusercontent.com/seudev/dockerized-apps/master/jaspersoft-studio/com.seudev.jaspersoft-studio.desktop -O ~/Desktop/com.seudev.jaspersoft-studio.desktop \
&& sed -i "s/<version>/5.5.1/" ~/.jaspersoft-studio/jaspersoft-studio \
&& sed -i "s/<user>/$USER/" ~/Desktop/com.seudev.jaspersoft-studio.desktop \
&& chmod 755 ~/.jaspersoft-studio/jaspersoft-studio ~/Desktop/com.seudev.jaspersoft-studio.desktop
```

**Note**: If you want to change the Jaspersoft Studio version, theme mode or other parameter, then open the `~/.jaspersoft-studio/jaspersoft-studio` file with a text editor and edit it.

## Using Dockerized Jaspersoft Studio

**Note**: If you has created a shortcut skip this section and just double click `Dockerized Jaspersoft Studio` shortcut, localizated in your desktop.

```sh
docker run -it --rm \
    --network host \
    -v ~/.jaspersoft-studio/JaspersoftWorkspace:/root/JaspersoftWorkspace \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix:ro \
    -e GTK_THEME=Adwaita:dark \
    seudev/jaspersoft-studio:5.5.1
```

### `docker run` parameters

| **parameter**                                 | **Description**                                                                             |
| --------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `--network host`                              | Use it if you need to access some database server directly on the host machine.             |
| `-v ~/.jaspersoft-studio/JaspersoftWorkspace` | Bind the `~/.jaspersoft-studio/JaspersoftWorkspace` local directory to cache the workspace. |
| `-e DISPLAY=$DISPLAY`                         | Set the `DISPLAY` environment variable with same local value. It's used by X server.        |
| `-v /tmp/.X11-unix:/tmp/.X11-unix:ro`         | Bind the `/tmp/.X11-unix` local directory in read only mode. It's used by X server.         |
| `-e GTK_THEME=Adwaita:light`                  | Set the light mode.                                                                         |
| `-e GTK_THEME=Adwaita:dark`                   | Set the dark mode.                                                                          |

### Building this image:

```sh
docker build -t seudev/jaspersoft-studio:5.5.1 .
```

## Licensing

**seudev/dockerized-apps** is provided and distributed under the [Apache Software License 2.0](http://www.apache.org/licenses/LICENSE-2.0).

Refer to *LICENSE* for more information.