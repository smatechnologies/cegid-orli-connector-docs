# Installation

The connector installation consists of multiple steps that are required to complete the installation successfully. This consists of installing a Windows Agent (if required), the Cegid ORLI Connector, configuring the connector and installing the Cegid Windows job subtype. 

It should be noted that the Cegid ORLI Connector requires a Windows Agent as it executes as a Windows batch job. It can be installed either on the central server or an agent can be installed on another Windows server.

## Supported Software Levels
The following software levels are required to implement the Cegid ORLI Connector.
- Cegid ORLI
    - Connector 25.0 version v22.
- OpCon Release 23.x or higher.
- Uses embedded Java 11.

## Installation
The installation process consists of the following steps:

- Cegid ORLI Connector Installation.
- Adding Cegid ORLI Connector job subtype to Enterprise Manager.
- Cegid ORLI Connector Configuration.

### Connector Installation
The Cegid ORLI Connector can be installed on the OpCon server or on a separate Windows server. When installing on a separate Windows server, it requires that an OpCon Windows Agent be installed on the server.

Copy the supplied install file CegidOrliConnector-win.zip file to the required windows system in a temp directory (c:\temp) and extract the files into the required location.

After the installation is complete, the installation root directory contains the connector executable, the encryption software module, the Connector.config file, a java directory containing the required Java environment and an emplugins directory containing the Job Sub type.

Installation complete.

### Job Subtype Installation
Copy the Enterprise Manager plug-in from the installation /emplugins directory to the dropins directory of the Enterprise Manager installation. 
If the dropins directory does not exist, create the dropins directory off the root directory. 

Restart Enterprise Manager and a new Windows job subtype called Cegid ORLI will be visible.

If not restart Enterprise Manager using 'Run as Administrator'. After this Enterprise Manager can be used normally.

Create a global property **OrliPath** that contains the full path of the installation directory.

### Connector Configuration
The configuration of the Cegid ORLI Connector requires setting the required values in the Connector.config file. 
The Connector.config file contains information for the Cegid ORLI Connector that defines the address of the web services, the web services end point, the location of the BatchManage.wsdl file and the user code and password to use for accessing the web services. All password values placed in the configuration file must be encrypted using the Encrypt.exe utility provided with the connector

#### Encrypt Utility
The Encrypt utility uses standard 64 bit encryption.

Supports a -v argument and displays the encrypted value

On Windows, example on how to encrypt the value "abcdefg":
```
Encrypt.exe -v abcdefg
```

#### Configuration
Configure the Connector.config file in the installation directory setting the required information.
The Connector.config contains the following values

Property Name | Value
--------- | -----------
**[CONNECTOR]**                | header
**CONNECTOR_NAME**             | The name of the connector. This value should not change
**POLL_DELAY**                 | The time to wait before submitting the first status request when tracking the status of a started task (default 5 seconds).
**POLL_INTERVAL**              | The time to wait fos subsequent status requests (default 3 seconds).
**DEBUG**                      | The Connector supports a debug mode which can be enabled by setting the value to ON. The connector should be run with DEBUG disabled (OFF) and enabled (ON) when requested to capture an error condition. Value either ON or OFF (default OFF).
**[ORLI]**                     | header
**ORLI_TOKEN_URL**             | This defines the address where authentication requests are submitted.
**ORLI_TOKEN_CLIENT_ID**       | This client id used to retrieve the authentication token
**ORLI_WEB_SERVICES_ENDPOINT** | This defines the web services end point to be used when calling the web service.
**ORLI_USER**                  | The name of a user that will be included in the authentication request.
**ORLI_USER_PASSWORD**         | The password of the defined user . The value must be encrypted using the Encrypt.exe tool.

 
Example configuration file. 
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
