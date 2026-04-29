---
title: Installation
sidebar_label: Installation
description: "Install and configure the Cegid ORLI Connector on a Windows server so OpCon can submit and monitor jobs in the Cegid ORLI application."
tags:
  - Procedural
  - System Administrator
  - Installation
---

# Installation

The Cegid ORLI Connector installation consists of three steps: installing the connector files, adding the Cegid ORLI job subtype to Enterprise Manager, and configuring the connector.

## What is it?

The Cegid ORLI Connector is a Windows batch program that runs on a Windows Agent machine. It communicates with the Cegid ORLI application using REST APIs and returns job status and output to OpCon.

- Install this connector when you need OpCon to automate jobs in the Cegid ORLI commercial management application
- The connector can be installed on the OpCon server or on a separate Windows machine that has the Windows Agent installed

## Supported software versions

The following software versions are required to run the Cegid ORLI Connector:

| Component | Required version |
|---|---|
| Cegid ORLI | 22.x or higher (Connector 25.0 requires v22) |
| OpCon | 23.x or higher |
| Java | Embedded Java 11 (included — no separate installation required) |

## Install the connector

To install the connector, complete the following steps:

1. Copy the `CegidOrliConnector-win.zip` file to the target Windows machine in a temporary directory (for example, `c:\temp`).
2. Extract the zip file to the desired installation directory.

After extraction, the installation directory contains the following:

| Item | Description |
|---|---|
| Connector executable | The main connector program |
| `Encrypt.exe` | Utility for encrypting password values |
| `Connector.config` | Configuration file |
| `java/` | Embedded Java 11 runtime |
| `emplugins/` | Enterprise Manager job subtype plugin |

## Install the job subtype

To add the Cegid ORLI job subtype to Enterprise Manager, complete the following steps:

1. Copy the Enterprise Manager plugin file from the connector's `emplugins/` directory to the `dropins/` directory of your Enterprise Manager installation.
2. If the `dropins/` directory does not exist, create it in the Enterprise Manager root directory.
3. Restart Enterprise Manager. The **Cegid ORLI** job subtype appears in the Windows job subtype list.

**NOTE:** If the job subtype does not appear after restarting, close Enterprise Manager and reopen it using **Run as Administrator**. After this step, Enterprise Manager can be used normally.

## Create the global property

To configure the connector path, complete the following steps:

1. In OpCon, create a global property named `OrliPath`.
2. Set the value to the full path of the connector installation directory.

**NOTE:** If more than one Cegid ORLI Connector is installed on the same machine, create an additional global property with a unique name and update the **Connector Path** field in each job definition accordingly.

## Configure the connector

The connector reads its settings from the `Connector.config` file in the installation directory. All password values must be encrypted using `Encrypt.exe` before being placed in the file.

### Encrypt a password value

To encrypt a password value, complete the following steps:

1. Open a command prompt in the connector installation directory.
2. Run the following command, replacing `yourpassword` with the value to encrypt:

```
Encrypt.exe -v yourpassword
```

3. Copy the encrypted output and paste it into the appropriate field in `Connector.config`.

### Configuration settings

The `Connector.config` file contains the following settings:

| Setting | Description | Default |
|---|---|---|
| **[CONNECTOR]** | | |
| `CONNECTOR_NAME` | The display name of the connector | `Cegid Orli` |
| `POLL_DELAY` | Seconds to wait before submitting the first status request after starting a job | `5` |
| `POLL_INTERVAL` | Seconds to wait between subsequent status requests | `3` |
| `DEBUG` | Enables detailed logging when set to `ON`. Set to `OFF` during normal operation | `OFF` |
| **[ORLI]** | | |
| `ORLI_TOKEN_URL` | The URL where authentication token requests are submitted | — |
| `ORLI_TOKEN_CLIENT_ID` | The client ID used to retrieve the authentication token | — |
| `ORLI_WEB_SERVICES_ENDPOINT` | The Cegid ORLI REST API endpoint for job requests | — |
| `ORLI_USER` | The username included in the authentication request | — |
| `ORLI_USER_PASSWORD` | The password for the defined user. Must be encrypted using `Encrypt.exe` | — |

### Example configuration file

```
[CONNECTOR]
CONNECTOR_NAME=Cegid Orli
POLL_DELAY=5
POLL_INTERVAL=3
DEBUG=OFF

[ORLI]
ORLI_TOKEN_URL=test.fr/orli-auth/realms/Cegid-Orli/protocol/openid-connect/token
ORLI_TOKEN_CLIENT_ID=orli-TEST-CLIENT
ORLI_WEB_SERVICES_ENDPOINT=test.fr/orli-ws/ORTS2/soap/v1/BatchManage
ORLI_USER=usera
ORLI_USER_PASSWORD=**********************
```

## FAQs

**Can I install the connector on a machine other than the OpCon server?**

Yes. The connector can be installed on any Windows machine that has the Windows Agent installed and configured in OpCon. The connector runs as a Windows batch job under the Windows Agent.

**What happens if the `dropins/` directory does not exist in Enterprise Manager?**

Create the directory manually in the Enterprise Manager root directory, then copy the plugin file into it and restart Enterprise Manager.

**Do I need to install Java separately?**

No. The connector includes an embedded Java 11 runtime in the `java/` subdirectory. No separately installed Java version is required.

**Why must passwords be encrypted?**

The `Connector.config` file is stored as plain text. Encrypting password values prevents credentials from being exposed in the file. Use the included `Encrypt.exe` utility to produce the encrypted value.

**What does `POLL_DELAY` and `POLL_INTERVAL` control?**

After submitting an executeRequest job, the connector polls the Cegid ORLI application to check when the job completes. `POLL_DELAY` sets how long to wait before the first status check, and `POLL_INTERVAL` sets how long to wait between each subsequent check.

## Glossary

**Connector** — The Cegid ORLI Connector executable that OpCon calls as a Windows batch job to communicate with the Cegid ORLI application.

**Connector.config** — The configuration file in the connector installation directory that defines connection settings, authentication credentials, and runtime behavior.

**Encrypt.exe** — A utility included with the connector that encrypts password values using 64-bit encryption for use in `Connector.config`.

**emplugins** — The directory containing the Enterprise Manager job subtype plugin. Copy its contents to the Enterprise Manager `dropins/` directory to enable the Cegid ORLI job subtype.

**OrliPath** — The OpCon global property that stores the full path to the connector installation directory. The job subtype uses this property to locate the connector executable at run time.

**Related topics:**

- [Overview](./overview.md)
- [Operation](./operation.md)
