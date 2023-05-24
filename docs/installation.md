# Installation

The connector installation consists of multiple steps that are required to complete the installation successfully. This consists of installing a Windows Agent (if required), the Cegid ORLI Connector, configuring the connector and installing the Cegid Windows job subtype. 

It should be noted that the Cegid ORLI Connector requires a Windows Agent as it executes as a Windows batch job. It can be installed either on the central server or an agent can be installed on another Windows server.

## Supported Software Levels
The following software levels are required to implement the Cegid ORLI Connector.
- Cegid ORLI
    - Connector 21.0.0 version V13, V14 & V15.
    - Connector 22.0.0 version v22.
- OpCon Release 19.x or higher.
- Uses embedded Java 11.

## Installation
The installation process consists of the following steps:

- Cegid ORLI Connector Installation.
- Adding Cegid ORLI Connector job subtype to Enterprise Manager.
- Cegid ORLI Connector Configuration.

### Connector Installation
The Cegid ORLI Connector can be installed on the OpCon server or on a separate Windows server. When installing on a separate Windows server, it requires that an OpCon Windows Agent be installed on the server.

Copy the supplied install file CegidOrliConnector-win.zip file to the required windows system in a temp directory (c:\temp) and extract the files into the required location.

After the installation is complete, the installation root directory contains the connector executable, the encryption software module, the Connector.config file, a java directory containing the required Java environment, a wsdl directory containing the BatchManage.wsdl file and an emplugins directory containing the Job Sub type.
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
**DEBUG**                      | The Connector supports a debug mode which can be enabled by setting the value to ON. The connector should be run with DEBUG disabled (OFF) and enabled (ON) when requested to capture an error condition. Value either ON or OFF (default OFF).
**[ORLI]**                     | header
**ORLI_SERVER_ADDRESS**        | This defines the address of the web server that supports the Orli web services. The value here is used to overwrite the web server address in the supplied wsdl.
**ORLI_WEB_SERVICES_ENDPOINT** | This defines the web services end point to be used when calling the web service.
**ORLI_ORLI_WSDL_LOCATION**    | This defines the location of the BatchManage.wsdl file relative to the installation directory. This value should not be changed.
**ORLI_USER**                  | The web server requires authentication and this is the name of a user that has the required privileges to access the web services.
**ORLI_USER_PASSWORD**         | The password of the defined user . The value must be encrypted using the Encrypt.exe tool.

 
Example configuration file. 
```
[CONNECTOR]
CONNECTOR_NAME=Cegid Orli
DEBUG=OFF

[ORLI]
ORLI_SERVER_ADDRESS=http://srv-roa-dvpux.cegidgroup.local:8001
ORLI_WEB_SERVICES_ENDPOINT=/orliweb/webservices/TRUNK/soap/BatchManage
ORLI_WSDL_LOCATION=wsdl/BatchManage.wsdl
ORLI_USER=orli
ORLI_USER_PASSWORD=6233426a6232353463484d3d

```
