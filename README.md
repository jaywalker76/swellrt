# SwellRT [![Build Status](https://travis-ci.org/P2Pvalue/swellrt.svg?branch=master)](https://travis-ci.org/P2Pvalue/swellrt) [![Gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg)](https://gitter.im/P2Pvalue/swellrt)

SwellRT is an open source **backend-as-a-service**. It allows to develop Web and _mobile_(*) apps faster by providing a set of common backend features:


### Features:

* Real-time storage (NoSQL)
* User management
* Auth
* Event based integration

SwellRT enables easily real-time collaboration in your apps:

* Collaborative text editing
* Chats
* Reactive user interface
* Push notifications
* Ubiquitous User Experience across devices


**Federation:**

SwellRT servers can be federated using Matrix.org open protocol.


_(*) Note: native mobile clients are not still available_

## Running SwellRT Server

You can build the server yourself or use our pre-built Docker image. 

### Building on your own

Prerequisites

- [Java JDK 8](http://openjdk.java.net/install/)
- [MongoDB 2.4](https://docs.mongodb.com/manual/administration/install-community/)

Clone the project

```
git clone git@github.com:P2Pvalue/swellrt.git
cd swellrt
```

Build

```
./gradlew compileJava devWeb
```

Start the server

```
./gradlew run
```

Visit "http://localhost:9898" to check server installation and try some demos.


### Docker

A docker image of the latest release of SwellRT is provided at [Docker Hub](https://hub.docker.com/r/p2pvalue/swellrt/):

```sh
$ docker pull p2pvalue/swellrt
```

This image doesn't include a MongoDB server. Setup mongo instance by pulling the latest release of mongo image:
```sh
$ docker pull mongo
```

To pass connection data for a MongoDB instance to the container pass following environment variables to the container:

```sh
$ docker run -e MONGODB_HOST=<host> -e MONGODB_PORT=<port> -e MONGODB_DB=<db name> -p 9898:9898 -h swellrt -d p2pvalue/swellrt
```

Sample Setup: 

```sh
$ docker run -p 27017:27017 --name mongo -d mongo

$ docker run -e MONGODB_HOST=mongo -e MONGODB_PORT=27017 -e MONGODB_DB=swellrt  -p 9898:9898 -h swellrt --name swellrt --link mongo:mongo -d p2pvalue/swellrt
```

Some SwellRT's configuration and data files must be stored out of the Docker container to avoid losing data when the image is updated. Map these folders in your host machine use following parameters:

```sh
$ docker run -v <host machine folder>:<docker image folder>  -p 9898:9898 -h swellrt -d p2pvalue/swellrt
```

List of docker's image folders that should be placed outside the container (in your host machine):

- `/usr/local/swellrt/config` Folder storing all config files. See documentation for details.
- `/usr/local/swellrt/log` Folder storing server log.
- `/usr/local/swellrt/sessions` Folder storing Web sessions.
- `/usr/local/swellrt/avatars` Folder storing user's avatar.
- `/usr/local/swellrt/attachments` Folder storing attachment files (when MongoDB is not used as attachment storage).

Visit "http://localhost:9898" to check server installation and try some demos.


## API (JavaScript Web)

At the moment, SwellRT only provides a JavaScript client for Web projects.

A good starting point is the source code of [form demo](https://github.com/P2Pvalue/swellrt/blob/master/wave/webapp/demo-form.html) and [text editor demo](https://github.com/P2Pvalue/swellrt/blob/master/wave/webapp/demo-pad.html)

#### Basic Concepts

The aim of SwellRT API is to provide easy programming of real-time collaboration. The programming model is based in two abstractions:


**Collaborators**

Users registered in any SwellRT server. Also there is a special _anonymous_ collaborator.
Each collaborator belongs to a server instance, and their ID has the syntax _(user name)@(server name)_


**Collaborative Objects**

A collaborative object is a data structure shared by one or more _collaborators_.
Each collaborator can have different level of access to an object and its parts.

An object, when it is used by a collaborator has three different data zones:

- Persisted zone: any change in this zone is automatically sync and persisted among collaborators.
- Private zone: only the current collaborator has access to this zone. This data is persisted.
- Transient zone*: any change in this zone is automatically among collaborators, but it is never persisted.

Each zone is a nested structured of arrays, maps and primitive values. For example, in Javascript it can be seen as
an object.

_(*) Transient zones are not still available._

## First steps


Ensure that SwellRT Server is running on "http://localhost:9898". To make your Web project load the client:

```
<script src='http://localhost:9898/swellrt-beta.js'></script>
```

Head over to our [GH Wiki](https://github.com/P2Pvalue/swellrt/wiki/Get-Started#basic-steps) for a quick jumpstart! 

## Contact and Support

Visit our [Gitter Channel](https://gitter.im/P2Pvalue/swellrt) or email to swellrt@gmail.com.

## Contributing

Read our [contributing guide](https://github.com/prastut/swellrt/blob/install-docs-fix/CONTRIBUTING.MD) to learn more about our development process, how to propose bugfixes and improvements, and how to build and test your changes to SwellRT.

## Acknowledgments

The SwellRT project is a fork of [Apache Wave](http://incubator.apache.org/wave/).
Initial work of SwellRT has been funded by the EU research project [P2Pvalue](http://p2pvalue.eu) and supported by [GRASIA research group](http://grasia.fdi.ucm.es/) of the *Universidad Complutense of Madrid*.


## Cryptographic Software Notice

This distribution includes cryptographic software.  The country in
which you currently reside may have restrictions on the import,
possession, use, and/or re-export to another country, of
encryption software.  BEFORE using any encryption software, please
check your country's laws, regulations and policies concerning the
import, possession, or use, and re-export of encryption software, to
see if this is permitted.  See <http://www.wassenaar.org/> for more
information.

The U.S. Government Department of Commerce, Bureau of Industry and
Security (BIS), has classified this software as Export Commodity
Control Number (ECCN) 5D002.C.1, which includes information security
software using or performing cryptographic functions with asymmetric
algorithms.  The form and manner of this Apache Software Foundation
distribution makes it eligible for export under the License Exception
ENC Technology Software Unrestricted (TSU) exception (see the BIS
Export Administration Regulations, Section 740.13) for both object
code and source code.

The following provides more details on the included cryptographic
software:

  Wave requires the BouncyCastle Java cryptography APIs:
    http://www.bouncycastle.org/java.html
