# Release Notes Cegid Orii 25.0

## General

The release supports Cegid Orli version 25.1.

This is a complete rewrite of the Cegid Orli connector removing the SOAP web services requests replacing them with 
Rest-API calls using SOAP data structures. 

Authentication is performed by retrieving an authorization token and then setting the value in the header of the Rest-API call.

The information is passed using a POST method and using XML soapenv Envelopes.

Returned XML data is parsed using the XPATH library.

## Migration Considerations

The configuration has changed and requires the new URL's as well as the authentication information required to retrieve a token for subsequent use.

# Release Notes Cegid Orii 22.1.0

## General

The release supports Cegid ORLI version 22.1.

## Migration Considerations

### New Features

### Fixes

**CONNUTIL-607**    

Fixed a problem in that Debug logging is not turned off even if config file is set to false.

# Release Notes Cegid ORLI 22.0.0

## General

The release supports Cegid ORLI version 22.0 (includes support for wsdl v2).

## Migration Considerations

This release no longer requires that a user is specified when calling the application. Instead the user / password values 
used for authentication are now used to execute the task. 

### New Features

### Fixes

**CONNUTIL-603**    

Update connector to support wsdl v2.

# Release Notes Cegid Orii 21.0.0

## General

The release removes log4j and replaces it with slj4j and logback.

## Migration Considerations

This release includes the new format installer where the files are extracted from the zip file into the desired directory. 
It contains an embedded java version for the connector so there is no reliance on installed Java versions.

The configuration file has been changed from Agent.config to Connector.config.

The connector implements new encryption capabilities and the documentation explains how to use the new Encrypt.exe mechanism.

### New Features

### Fixes

**CONNUTIL-534**    
                    
Cegid ORLI log file does not switch on defined values.

**CONNUTIL-542**    

CVE-2021-44228 adjustment removing log4j as the logging component.
			
