Ghost - Docker
==============

Run Ghost with Docker.

Basic Usage
-----------

```bash
$ docker run -i -t -p 2398:2368 ghost
```

### Custom config, persistent data

The startup script will check whether the following files or directories exist:
- ```/data/config.js```
- ```/data/content/apps```
- ```/data/content/data```
- ```/data/content/images```
- ```/data/content/themes```

The existing files and directories will be used, otherwise, Ghost will fallback
to the default paths.

#### Note for SELinux users

If your distribution runs SELinux, you may need to use the following command in
order to mount volumes in your docker images:

```bash
$ chcon -Rt svirt_sandbow_file_t <your_directory>
```

#### Recommended way

Assuming ```your_directory_of_choice``` contains:
- ```config.js```: the configuration file
- ```content```
    - ```content/apps```: (not used in current versions of Ghost...)
    - ```content/data```: the location for data (blog posts, settings, etc.)
    - ```content/images```: the location for images
    - ```content/themes```: the location for themes

```bash
$ cd <your_directory_of_choice>
$ docker run -i -t -v $(pwd):/data -p 2368:2368 ghost
```

#### Example

```bash
$ cd example
$ docker run -i -t -v $(pwd):/data -p 2368:2368 ghost
```

To clean the example:
```bash
$ git reset --hard && git clean -f
```


#### Separate locations
It is also possible to only mount specific files or directories. For instance:

```bash
$ docker run -i -t -v <your_config>:/data/config.js -p 2368:2368 ghost
$ docker run -i -t -v <your_config>:/data/config.js <your_data>:/data/content/data -p 2368:2368 ghost
```

Why not use [DockerfileGhost](dockerfile/ghost)?
------------------------------------------------

The dockerfiles from the [DockerfileProject][] are fine, but most of them use
versions dubbed “latest” instead of building against specific versions.

Building Docker images against the latest versions of softwares is fine, as long
as the version is correctly targeted: if the current latest version of a software
is 1.9, use ```software-1.9``` instead of ```software-latest```. This way, the
Docker images are immune to unexpected new releases (```software-latest``` shifting
from ```software-1.9``` to ```software-2.0```...).

[DockerfileGhost]: https://dockerfile.github.io/#/ghost
[DockerfileProject]: https://dockerfile.github.io
