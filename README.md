# Data Provenance in FIWARE Architectures[<img src="https://img.shields.io/badge/NGSI-LD-d6604d.svg" width="90"  align="left" />](https://www.etsi.org/deliver/etsi_gs/CIM/001_099/009/01.07.01_60/gs_cim009v010701p.pdf)[<img src="https://fiware.github.io/tutorials.Big-Data-Flink/img/fiware.png" align="left" width="162">](https://www.fiware.org/)<br/>

[![FIWARE Core Context Management](https://nexus.lab.fiware.org/static/badges/chapters/core.svg)](https://github.com/FIWARE/catalogue/blob/master/processing/README.md)
[![License: MIT](https://img.shields.io/github/license/fiware/tutorials.Big-Data-Spark.svg)](https://opensource.org/licenses/MIT)
[![Support badge](https://nexus.lab.fiware.org/repository/raw/public/badges/stackoverflow/fiware.svg)](https://stackoverflow.com/questions/tagged/fiware)

FIWARE Step-by-Step Tutorial: Data Provenance using NGSI-LD and FIWARE Context Broker

This tutorial is an introduction to the [FIWARE CanisMajor](https://fiware.github.io/CanisMajor), is a blockchain 
adaptor that supports various Distributed Ledger Technology (DLT), the adaptor aims to submit the data to DLT using 
FIWARE Technologies. DLT is the technological infrastructure and protocols that allow validation, 
simultaneous access, validation, and record updating across a distributed database. Blockchain is based on 
DLT and allows users view any change and who made it, reducing the need of auditing data and ensuring that 
data is reliable, with access control to this data.


The tutorial uses [cUrl](https://ec.haxx.se/) commands throughout, but is also available as
Postman documentation](...)

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/0a602cbb6bbf9351efc2)
[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/FIWARE/tutorials.Big-Data-Flink/tree/NGSI-LD)

## Contents

<details>

<summary><strong>Details</strong></summary>

-   ...

</details>

## Data Provenance

The term “Data Provenance” refers to a record trail that accounts the origin of a data portion together with 
the information that explains how and why this data portion got to the present status. Example: In an application 
like Clean Water, a lot of data is derived from the status of the water, which in turn might be derived from 
original status after some water transformations, which are derived from water observations. A provenance 
record will keep this history for each piece of data and which operations were developed to achieve the current 
status of Water Quality.

## Architecture

It will make use of three FIWARE components - the
[Orion-LD Context Broker](https://fiware-orion.readthedocs.io/en/latest/), and the 
[CanisMajor](https://fiware.github.io/CanisMajor) for connecting Orion-LD to the Blockchain.

Orion-LD Context Broker relies on open source [MongoDB](https://www.mongodb.com/) technology to keep persistence 
of the information they hold.

The overall architecture can be seen below:

![](https://fiware.github.io/tutorials.Big-Data-Flink/img/architecture.png)

Since all interactions between the elements are initiated by HTTP requests, the entities can be containerized and run
from exposed ports.

The necessary configuration information can be seen in the services section of the associated `docker-compose.yml` file:

```yaml
orion:
    image: fiware/orion-ld
    hostname: orion
    container_name: fiware-orion
    depends_on:
        - mongo-db
    networks:
        - default
    ports:
        - "1026:1026"
    command: -dbhost mongo-db -logLevel DEBUG
    healthcheck:
        test: curl --fail -s http://orion:1026/version || exit 1
```

```yaml
mongo-db:
    image: mongo:3.6
    hostname: mongo-db
    container_name: db-mongo
    expose:
        - "27017"
    ports:
        - "27017:27017"
    networks:
        - default
    command: --nojournal
```

The necessary configuration information ...

## Prerequisites

### Docker and Docker Compose

To keep things simple, all components will be run using [Docker](https://www.docker.com). **Docker** is a container
technology which allows to different components isolated into their respective environments.

-   To install Docker on Windows follow the instructions [here](https://docs.docker.com/docker-for-windows/)
-   To install Docker on Mac follow the instructions [here](https://docs.docker.com/docker-for-mac/)
-   To install Docker on Linux follow the instructions [here](https://docs.docker.com/install/)

**Docker Compose** is a tool for defining and running multi-container Docker applications. A series of
[YAML files](https://github.com/FIWARE/tutorials.Big-Data-Flink/blob/NGSI-LD/docker-compose.yml) are used to configure
the required services for the application. This means all container services can be brought up in a single command.
Docker Compose is installed by default as part of Docker for Windows and Docker for Mac, however Linux users will need
to follow the instructions found [here](https://docs.docker.com/compose/install/)

You can check your current **Docker** version using the following commands:

```console
docker -v
```

Please ensure that you are using Docker version 24.0.6 or higher and upgrade if necessary.

### Java JDK

...

You can check your current **Java JDK** version using the following commands:

```console
java -v
```

Please ensure that you are using Java version 17.x or higher and upgrade if necessary.

### Maven

[Apache Maven](https://maven.apache.org/download.cgi) is a software project management and comprehension tool. Based on
the concept of a project object model (POM), Maven can manage a project's build, reporting and documentation from a
central piece of information. We will use Maven to define and download our dependencies and to build and package our
code into a JAR file.

You can check your current **Maven** version using the following commands:

```console
mvn -v
```

Please ensure that you are using Maven version 3.6.3 or higher and upgrade if necessary.

### Cygwin for Windows

We will start up our services using a simple Bash script. Windows users should download [cygwin](http://www.cygwin.com/)
to provide a command-line functionality similar to a Linux distribution on Windows.

## Start Up

Before you start, you should ensure that you have obtained or built the necessary Docker images locally. Please clone
the repository and create the necessary images by running the commands shown below. Note that you might need to run some
of the commands as a privileged user:

```console
git clone https://github.com/flopezag/tutorials.Data_Provenance
cd tutorials.Data_Provenance
```

To start the system, run the following command:

```console
./services [orion|scorpio|stellio]
```

> :information_source: **Note:** If you want to clean up and start over again you can do so with the following command:
>
> ```
> ./services stop
> ```

### List Operations...

Feature: Store transactions on entities in CanisMajor
  CUD operations on entities in the broker should be persisted in CanisMajor.

  Scenario: A test-store, created at orion-ld, is available through CanisMajor.
    Given CanisMajor is running and available for requests.
    And Vault is configured as a signing endpoint.
    And Franzi is registered in vault.
    And Mira is registered in vault.
    When Franzi creates the test-store.
    Then Only one transaction should be persisted for the entity.
    And The transaction to persist test-store can be read through CanisMajor.

  Scenario: Multiple entity creations are persisted in CanisMajor.
    Given CanisMajor is running and available for requests.
    And Vault is configured as a signing endpoint.
    And Franzi is registered in vault.
    And Mira is registered in vault.
    When Franzi creates the test-store.
    And  Mira creates another entity.
    Then All transactions should be in CanisMajor.
    And All transactions for Mira are presisted.
    And All transactions for Franzi are presisted.

  Scenario: When changes and queries happen, they are all persisted in CanisMajor.
    Given CanisMajor is running and available for requests.
    And Vault is configured as a signing endpoint.
    And Mira is registered in vault.
    And Franzi is registered in vault.
    When Franzi creates the test-store.
    And Mira updates the test store.
    And Mira retrieves the test store.
    And Mira queries for unicorns.
    And Mira queries for unicorns, providing a user id.
    Then All transactions should be in CanisMajor.
    And All transactions for Mira are presisted.

  Scenario: When multiple changes happen at an entity, they are all persisted in CanisMajor.
    Given CanisMajor is running and available for requests.
    And Vault is configured as a signing endpoint.
    And Franzi is registered in vault.
    When Franzi creates the test-store.
    And Franzi updates the test store.
    Then All transactions should be in CanisMajor.

  Scenario: When updates without wallet-information happen, the default account should be used.
    Given CanisMajor is running and available for requests.
    And Vault is configured as a signing endpoint.
    When Anonymous user creates a delivery.
    And Anonymous user updates a delivery.
    Then All transactions should be in CanisMajor.
    And All transactions for Default are presisted.

  Scenario: When updates with and without account happen, the default or the correct account should be used.
    Given CanisMajor is running and available for requests.
    And Vault is configured as a signing endpoint.
    And Mira is registered in vault.
    And Franzi is registered in vault.
    When Anonymous user creates a delivery.
    And Franzi creates the test-store.
    And Franzi updates the test store.
    And Mira updates the test store.
    And Anonymous user updates a delivery.
    Then All transactions should be in CanisMajor.
    And All transactions for Default are presisted.
    And All transactions for Franzi are presisted.
    And All transactions for Mira are presisted.

  Scenario: When create, update and upsert are used, all operations are persisted in the blockchain.
    Given CanisMajor is running and available for requests.
    When Anonymous user creates a delivery.
    And Anonymous user updates a delivery.
    When Anonymous upserts multiple deliveries.
    Then All transactions should be in CanisMajor.


# Next Steps

Want to learn how to add more complexity to your application by adding advanced features? You can find out by reading
the other [tutorials in this series](https://ngsi-ld-tutorials.rtfd.io)

---

## License

[MIT](LICENSE) © 2023 FIWARE Foundation e.V.
