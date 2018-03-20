# bouyguestelecom-smsapi
A Bash API for Bouygues Telecom SMS unofficial API (5 SMS /day limitation)

![Stable version](https://img.shields.io/badge/stable-1.1.0-blue.svg)
[![BSD-3 license](https://img.shields.io/badge/license-BSD--3--Clause-428F7E.svg)](https://tldrlegal.com/license/bsd-3-clause-license-%28revised%29)

*bouyguestelecom-smsapi* is a Bash library which allows to use Bouygues Telecom unofficial SMS API.

It's based on [bouyguessms](https://github.com/tomsquest/bouyguessms) program written in [Go](https://golang.org) by
[Thomas Queste](https://github.com/tomsquest).

## Bouygues Telecom SMS unofficial API

To be able to use this feature, you must have subscribed a mobile line with [Bouygues Telecom](https://www.bouyguestelecom.fr) operator.

With a Bouygues Telecom account, using their Web interface, you are allowed to send until 5 SMS per day.

It's on this possibility [bouyguessms](https://github.com/tomsquest/bouyguessms) is based.

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

| Name  | Meaning                                                  |
|:------|:---------------------------------------------------------|
| LOGIN | Bouygues Telecom login                                   |
| PASS  | Password associated to login                             |
| TO    | Target mobile phones numbers separated by: ';' character |

Example:
```bash
# Bouygues Telecom login
LOGIN="0606060606"
# Password associated to login
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

#### bouyguessms

[bouyguessms](https://github.com/tomsquest/bouyguessms) can be installed in two ways:

 * Using a release available here: [bouyguessms releases](https://github.com/tomsquest/bouyguessms/releases)
 
 * Compiled from sources and installed into `/usr/local/bin`:
    ```bash
    sudo apt install golang-go
    cat << EOF >> ~/.bashrc
    
    # Go environment
    export GOPATH=$HOME/go
    EOF
    ```
    Now start a new session in order to load `GOPATH` variable and run:
    ```bash
    go get github.com/tomsquest/bouyguessms
    cd $GOPATH/src/github.com/tomsquest/bouyguessms/cmd/bouyguessms
    go build
    sudo mv bouyguessms /usr/local/bin
    ```

#### bouyguestelecom-smsapi

A [Debian](https://www.debian.org) package is available at [http://packages.cyosp.com/debian](http://packages.cyosp.com/debian) for:
 * [Stretch](https://www.debian.org/releases/stretch/) version
 * amd64 architecture

Steps to install it are:

 * Receive CYOSP GPG key from key server:

    `sudo gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys D71923F61CC21365`

 * Add GPG key to apt:

    `sudo gpg -a --export D71923F61CC21365 | apt-key add -`

 * Add CYOSP repository:

    ```bash
    sudo cat << EOF > /etc/apt/sources.list.d/cyosp.list

    # CYOSP packages
    deb http://packages.cyosp.com/debian stretch main

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