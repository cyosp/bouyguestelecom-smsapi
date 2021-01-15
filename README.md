# bouyguestelecom-smsapi
A Bash API for Bouygues Telecom SMS unofficial API (5 SMS /day limitation)

![Stable version](https://img.shields.io/badge/stable-1.5.0-blue.svg)
[![BSD-3 license](https://img.shields.io/badge/license-BSD--3--Clause-428F7E.svg)](https://tldrlegal.com/license/bsd-3-clause-license-%28revised%29)

*bouyguestelecom-smsapi* is a Bash library which allows to use Bouygues Telecom unofficial SMS API.

It's based on [bouyguestelecom-sms](https://github.com/cyosp/bouyguestelecom-sms).

## Bouygues Telecom SMS unofficial API

To be able to use this feature, you must have subscribed a mobile line with [Bouygues Telecom](https://www.bouyguestelecom.fr) operator.

With a Bouygues Telecom account, using their Web interface, you are allowed to send until 5 SMS per day.

It's on this possibility [bouyguestelecom-sms](https://github.com/cyosp/bouyguestelecom-sms) is based.

And so this Bash API.

## Library files

This library is composed of two files:

 * `/etc/bouyguestelecom-smsapi/bouyguestelecom-smsapi.conf.src`

	*The configuration file*

 * `/usr/lib/bouyguestelecom-smsapi/bouyguestelecom-smsapi.bash.src`

	*The library*

Both are located inside the directory **bouyguestelecom-smsapi** which allows to build a Debian package.

## Configuration file

Configuration file: `/etc/bouyguestelecom-smsapi/bouyguestelecom-smsapi.conf.src` is in fact a file which is sourced by the Bash library.

Thus syntax is the Bash one and is composed of the following variables:

| Name     | Meaning                                                  | Mandatory |
|:---------|:---------------------------------------------------------|-----------|
| LASTNAME | Base64 Bouygues Telecom account last name                | false     |
| LOGIN    | Plain or base64 Bouygues Telecom login                   | true      |
| PASS     | Plain or base64 password associated to login             | true      |
| TO       | Target mobile phones numbers separated by: ';' character | true      |

Example:
```bash
# Base64 Bouygues Telecom account last name
LASTNAME="TGFzdCBuYW1lCg=="
# Plain or base64 Bouygues Telecom login
LOGIN="0606060606"
# Plain or base64 password associated to login
PASS="1a2B3c4D5e6F7g"
# Target mobile phones numbers separated by: ';' character
TO="0707070707"
```

## HOW TO

### Use

To use the library there are only two steps:

1. Source the library:
```bash
. /usr/lib/bouyguestelecom-smsapi/bouyguestelecom-smsapi.bash.src
```
2. Call the function to send a SMS:
```bash
callBouyguesTelecomSmsApi "My first SMS"
```

In this example *My first SMS* will be sent to the number 0707070707  which is defined in the configuration file.

Exit code will be `0` in case of success, `1` otherwise.

### Install

#### bouyguestelecom-sms

If you use *bouyguestelecom-smsapi* Debian package this step is not needed.

Otherwise, build and installation steps are described in [bouyguestelecom-sms](https://github.com/cyosp/bouyguestelecom-sms) repository.

#### bouyguestelecom-smsapi

A [Debian](https://www.debian.org) package is available at [http://packages.cyosp.com/debian](http://packages.cyosp.com/debian) for:
 * [Stretch](https://www.debian.org/releases/stretch/) and [Buster](https://www.debian.org/releases/buster/) version
 * `amd64` and `armhf` architectures

Steps to install it are:

 * Receive CYOSP GPG key from key server:

    `sudo gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 15205D0CC73A9DF1`

 * Add GPG key to apt:

    `sudo gpg -a --export 15205D0CC73A9DF1 | apt-key add -`

 * Add CYOSP repository:

    ```bash
    sudo cat << EOF > /etc/apt/sources.list.d/cyosp.list

    # CYOSP packages
    deb http://packages.cyosp.com/debian buster main

    EOF
    ```

 * Update repository database:

    `sudo apt update`

 * Install package:
 
    `sudo apt install bouyguestelecom-smsapi`

## Create the Debian package

You can create the Debian package with the following commands:

```bash
# Move to the repository directory
cd /github/bouyguestelecom-smsapi/repository/path
# Generate the Debian package
sudo dpkg-deb --build bouyguestelecom-smsapi
# Update the package name following Debian convention
VERSION=$(grep "Version" bouyguestelecom-smsapi/DEBIAN/control | cut -d ' ' -f 2)
ARCH=$(grep "Architecture" bouyguestelecom-smsapi/DEBIAN/control | cut -d ' ' -f 2)
mv bouyguestelecom-smsapi.deb bouyguestelecom-smsapi_${VERSION}_${ARCH}.deb
```

## License

**[bouyguestelecom-smsapi](https://github.com/cyosp/bouyguestelecom-smsapi)** is released under the BSD 3-Clause License.

See the bundled `LICENSE.md` for details.
