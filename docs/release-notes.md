# Release Notes Cegid OrLi 21.0

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
                    Cegid Orli log file does not switch on defined values.
**CONNUTIL-542**    
                    CVE-2021-44228 adjustment removing log4j as the logging component.
			