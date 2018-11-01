![logo](https://raw.githubusercontent.com/unifiedstreaming/origin/master/unifiedstreaming-logo-black.png)

# Unified Remix

This repository demonstrates how to use Unified Remix to process playlists which
can be served by Unified Origin. It uses Docker to remove the need to install
any additional software.

For more details see [Unifed Remix](http://www.unified-streaming.com/products/unified-remix).

For more use cases see [Remix Demo](http://demo.unified-remix.com/).

## Example Content

This repository includes an example SMIL playlist which concatenates a Unified
Remix logo with the first 60 seconds of Big Buck Bunny.

The SMIL and media files are included in the content directory.


## Licensing

You need a license key to use this software. To evaluate you can create an account at [Unified Streaming Registration](https://private.unified-streaming.com/register/).

The license key is passed to containers using the *USP_LICENSE_KEY* environment variable.


## Setup

1. Install [Docker](http://docker.io)
2. Clone this repository
3. Run ./pull_images.sh

## Usage

Run the Unified Origin:

```bash
#!/bin/sh
export USP_LICENSE_KEY=<your_license_key>
docker run \
  -e USP_LICENSE_KEY \
  -v $(pwd)/content:/var/www/unified-origin \
  -p 80:80 \
  -d \
  unifiedstreaming/origin:1.9.5
```

### Prepare content

Process Remix playlist (replacing <your_license_key\> with your license key string):

```bash
#!/bin/sh
export USP_LICENSE_KEY=<your_license_key>
docker run \
  -e USP_LICENSE_KEY \
  -v $(pwd)/content:/data \
  unifiedstreaming/packager:1.9.5 \
  unified_remix \
    --license-key=$USP_LICENSE_KEY \
    -o /data/example.mp4 \
    /data/example.smil
```

Package ISM (replacing <your_license_key\> with your license key string, and <packager_options\> with any additional options you require, e.g. DRM):

```bash
#!/bin/sh
export USP_LICENSE_KEY=<your_license_key>
export PACKAGER_OPTIONS=<packager_options>
docker run \
  -e USP_LICENSE_KEY \
  -v $(pwd)/content:/data \
  unifiedstreaming/packager:1.9.5 \
    -o /data/example.ism \
    $PACKAGER_OPTIONS \
    /data/example.mp4
```

### Play content

The Remixed stream can now be played from the Origin as normal:

* HLS: http://<host_ip\>/example.ism/.m3u8
* DASH: http://<host_ip\>/example.ism/.mpd
* Smoothstreaming: http://<host_ip\>/example.ism/Manifest