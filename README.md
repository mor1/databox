# Databox

The Databox platform is an open-source personal networked device, augmented by
cloud-hosted services, that collates, curates, and mediates access to an
individual’s personal data by verified and audited third-party applications and
services. The Databox will form the heart of an individual’s personal data
processing ecosystem, providing a platform for managing secure access to data
and enabling authorised third parties to provide the owner with authenticated
services, including services that may be accessed while roaming outside the home
environment. Databox project is led by Dr Hamed Haddadi (Imperial College) in
collaboration with [Dr Richard Mortier](http://mort.io/) (University of
Cambridge) and Professors Derek McAuley, Tom Rodden, Chris Greenhalgh, and Andy
Crabtree (University of Nottingham) and funded by EPSRC. See
<http://www.databoxproject.uk/> for more information.

## Getting Started

We currently support Linux and Mac OS X. Running on other platforms is possible
using a virtual machine running Linux with _bridge mode_ networking. Also note
that more than one CPU core must be allocated to the VM.

### Prerequisites

1. Install [Docker](https://docker.com/) following the [instructions for your
   platform](https://docs.docker.com/engine/installation/), and ensure it is
   running.
2. Install [Docker Compose](https://docs.docker.com/compose/) following the [instructions for your platform](https://docs.docker.com/compose/install/).
3. Install [Git](https://git-scm.com) if it is not already on your machine, following the [instructions for your platform](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
4. Clone the [Databox git repo](https://github.com/me-box/databox/):
```
git clone https://github.com/me-box/databox.git
```

### Operation

Start your Databox by pulling pre-built images from the [Docker hub](<https://hub.docker.com/r/databoxsystems>) and running them:

```
cd databox
./databox-start
```

Point your web browser at <http://127.0.0.1> and follow the instructions to
configure your HTTPS certificates to access Databox UI securely using a web
browser <https://127.0.0.1>, or the iOS and Android app.

> Note: Using the databox iOS and Android apps with MacOS may require you to
> modify your firewall to enable external access to port 80 and 443.

To run the Databox tests, [described here](./TESTING.md):

```
./databox-test

```

Stop Databox and clean up:

```
./databox-stop
```

## Development

Databox comprises several core platform components required for Databox
function, plus a range of optional components in the form of **apps**
(encapsulating on-Databox data processing) and **drivers** (effectively apps
with extra privileges allowing them to interact off-Databox).

Overall system design can be find [here](./documents/system_overview.md) and
general API specifications are [here](./documents/api_specification.md).

### Building Apps with the Graphical SDK

The graphical SDK lets you quickly build and test simple Databox apps. To start
the SDK run:

```
./databox-start sdk
```

Visit <http://127.0.0.1:8086> to access the SDK web UI. Stop the SDK by running

```
./databox-stop sdk
```

### Building Apps and Drivers from Scratch

For those who don't want to develop using the SDK, we provide [Databox
API](./documents/api_specification.md) support libraries for several languages,
currently:

+ [Python](https://github.com/me-box/lib-python-databox)
+ [Go](https://github.com/me-box/lib-go-databox)
+ [Node.js](https://github.com/me-box/node-databox)

You do not need to run the Databox in `dev` mode to develop apps and drivers.
All you need is a `Dockerfile` and a `databox-manifest.json`. Examples can be
found in the libraries `/samples` directories. To install your app locally for
testing, upload the manifest to <http://127.0.0.1:8181> and build the image via
`docker build -t [your-app-name] .`.

To hack on a currently available driver or app, download and build the code on
your machine and upload the Databox manifest to your local app store:

```
./databox-install-component DRIVER-OR-APP
```

For example,

```
./databox-install-component driver-os-monitor
```

This will also work with your repositories and forks:

```
./databox-install-component [GITHUB_USERNAME]/[GITHUB_REPONAME]
```

Sample apps include:

* [app-light-graph](https://github.com/me-box/app-light-graph), plotting phone
  light sensor data
* [app-twitter-sentiment](https://github.com/me-box/app-twitter-sentiment),
  calculates sentiment across Twitter data
* [app-os-monitor](https://github.com/me-box/app-os-monitor), plots the output
  of the platform performance data

Sample drivers include:

* [driver-sensingkit](https://github.com/me-box/driver-sensingkit), providing
  mobile sensor data via SensingKit
* [driver-google-takeout](https://github.com/me-box/driver-google-takeout),
  supporting bulk import of Google Takeout data
* [driver-phillips-hue](https://github.com/me-box/driver-phillips-hue), enabling
  connection to the Phillips Hue Platform
* [driver-os-monitor](https://github.com/me-box/driver-os-monitor), monitoring
  the Databox hardware in terms of memory consumption and CPU load
* [driver-twitter](https://github.com/me-box/driver-twitter), streams data from
  a Twitter account
* [driver-tplink-smart-plug](https://github.com/me-box/driver-tplink-smart-plug),
collecting data from TP-Link smart plugs

### Developing Core Components

To develop  core components, you must run the Databox in `dev` mode:

```
./databox-start dev
```

Rather than use the pre-built images, this will clone all relevant source
repositories locally, and build the requisite Docker images for the core
platform components.

* [core-arbiter](https://github.com/me-box/core-arbiter), manages dataflow by
  minting tokens and controlling store discovery
* [core-container-manager](https://github.com/me-box/core-container-manager),
  controls build, installation and running functions of all other components
* [core-export-service](https://github.com/me-box/core-export-service),
  controls data to be exported to external URLs
* [core-network](https://github.com/me-box/core-network), controls connectivity
  between all components
* [core-store](https://github.com/me-box/core-store), provides storage for apps
  and drivers to store and retrieve JSON data and JPEG images
* [platform-app-server](https://github.com/me-box/platform-app-server), stores
  and serves Databox app and driver manifests

## Contributing

We welcome pull requests and other contributions -- see our [list of
contributors](https://github.com/me-box/databox/contributors)! [See
here](./CONTRIBUTING.md) for more information, and [contributors]in this
project. Potentially good starting points can be found in the [issue
tracker](https://github.com/me-box/databox/issues), and begin by
[forking](https://github.com/me-box/databox#fork-destination-box) the Databox
repository. All code is licensed under the [MIT Licence](./LICENSE).

## Acknowledgements

Development of Databox is supported by the [EPSRC](http://epsrc.ac.uk/) through
the following grants:

* EP/N028260/1, Databox: Privacy-Aware Infrastructure for Managing Personal Data
* EP/N028260/2, Databox: Privacy-Aware Infrastructure for Managing Personal Data
* EP/N014243/1, Future Everyday Interaction with the Autonomous Internet of
  Things
* EP/M001636/1, Privacy-by-Design: Building Accountability into the Internet of
  Things (IoTDatabox)
* EP/M02315X/1, From Human Data to Personal Experience
