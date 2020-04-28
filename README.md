# Libvips AWS Lambda Layer

:warning: This repository is no longer maintained. See https://github.com/jcupitt/docker-builds for the best libvips Dockerfile implementation.

---

Fork of amazing [ruby-vips-lambda](https://github.com/customink/ruby-vips-lambda) project by [customink](https://github.com/customink) with small Golang-related changes.

## Usage

* Clone or fork this repository.
* Make sure you have Docker and AWS CLI installed.
* Run `./bin/deploy`

From there you simply use the arn in your AWS SAM `template.yaml` file.


## Methodology

Simplicity and small file size! We followed the [docs](https://libvips.github.io/libvips/install.html) for `libvips` install. But because AWS Lambda already has ImageMagick and lots of the needed dependencies, the work was very basic.

We used the `lambci/lambda:build-go1.x` Docker image from the [docker-lambda](https://github.com/lambci/docker-lambda) project. From there we only had to install a few more dependencies to get libvips installed. The current version is `v8.8.4` and easy to configure if you need something else.

Lastly, we were happy to find that `glib` and `gobject` were already installed and all that was needed were some simple sym links so FFI could load these libraries.


## Contents

Because of the way we build `libvips` by using existing libraries already installed on AWS Lambda, the resulting layer is very small. Only around `9MB` in total un-compressed size.

```shell
$ ls -lAGp /opt/lib
total 9517
lrwxrwxrwx 1 root      27 Apr 15 20:46 libglib-2.0.so -> /usr/lib64/libglib-2.0.so.0
lrwxrwxrwx 1 root      30 Apr 15 20:46 libgobject-2.0.so -> /usr/lib64/libgobject-2.0.so.0
lrwxrwxrwx 1 root      17 Apr 15 20:46 libvips.so -> libvips.so.42.9.5
lrwxrwxrwx 1 root      17 Apr 15 20:46 libvips.so.42 -> libvips.so.42.9.5
-rwxr-xr-x 1 root 9745021 Apr 15 20:45 libvips.so.42.9.5
```




