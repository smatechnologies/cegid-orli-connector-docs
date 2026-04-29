---
title: Cegid ORLI Connector release notes
sidebar_label: Release notes
description: "Release notes for the Cegid ORLI Connector, listing new features, improvements, and bug fixes by version."
tags:
  - Reference
  - System Administrator
  - Automation Engineer
---

# Cegid ORLI Connector release notes

## 25

### 25.0

2025

### What's new

:eight_spoked_asterisk: **Complete rewrite using REST API with SOAP data structures.** The connector no longer uses SOAP web services. All requests now use REST API calls with SOAP data structures passed via XML SOAP envelopes.

:eight_spoked_asterisk: **Token-based authentication.** The connector now retrieves an authorization token from the configured token URL and sets it in the REST API request header. Previous versions used direct SOAP web service credentials.

:eight_spoked_asterisk: **Updated configuration.** The `Connector.config` file now requires token URL and client ID values (`ORLI_TOKEN_URL`, `ORLI_TOKEN_CLIENT_ID`) in addition to the web services endpoint and user credentials.

### Why this matters

The rewrite from SOAP web services to REST API aligns the connector with current Cegid ORLI API standards. Token-based authentication improves security by separating credential management from individual requests. Existing installations require updated configuration values for the new authentication and endpoint fields before upgrading.

### Upgrade notes

- Update `Connector.config` with the new `ORLI_TOKEN_URL` and `ORLI_TOKEN_CLIENT_ID` values before running jobs
- The `ORLI_WEB_SERVICES_ENDPOINT` value must be updated to the REST API endpoint
- Re-encrypt the `ORLI_USER_PASSWORD` value using the updated `Encrypt.exe` tool if needed

---

## 22

### 22.1.0

### What's new

No new features in this release.

### Fixes

:white_check_mark: **CONNUTIL-607**: Fixed an issue where debug logging remained active even when the `DEBUG` setting in `Connector.config` was set to `OFF`.

---

### 22.0.0

### What's new

:eight_spoked_asterisk: **Support for Cegid ORLI wsdl v2.** The connector now supports the version 2 WSDL for Cegid ORLI web services.

### Why this matters

The connector now works with Cegid ORLI environments running the updated wsdl v2 interface.

### Upgrade notes

- This release no longer requires a user to be specified when calling the application. The user and password values in the configuration file are used for authentication and job execution. Remove any user specification from existing job definitions.

### Fixes

:white_check_mark: **CONNUTIL-603**: Updated the connector to support wsdl v2.

---

## 21

### 21.0.0

### What's new

:eight_spoked_asterisk: **Replaced log4j with slf4j and logback.** The connector no longer uses log4j for logging. All logging now uses slf4j with logback.

:eight_spoked_asterisk: **Embedded Java runtime.** The connector now includes an embedded Java 11 runtime. No separately installed Java version is required on the target machine.

:eight_spoked_asterisk: **New installer format.** The connector is now distributed as a zip file. Extract the zip to the desired installation directory.

:eight_spoked_asterisk: **New Encrypt.exe utility.** A new encryption utility is included for encrypting password values in `Connector.config`. The configuration file has been renamed from `Agent.config` to `Connector.config`.

### Why this matters

Removing log4j addresses CVE-2021-44228 and eliminates the Log4Shell vulnerability. The embedded Java runtime simplifies installation by removing the dependency on a system-installed Java version. The new installer format makes deployment more straightforward.

### Upgrade notes

- Rename existing `Agent.config` to `Connector.config` and verify all settings are present
- Use the new `Encrypt.exe` tool to re-encrypt any password values
- No separately installed Java runtime is required

### Fixes

:white_check_mark: **CONNUTIL-534**: Fixed an issue where the Cegid ORLI log file did not rotate at the configured file size limit.

:white_check_mark: **CONNUTIL-542**: Removed log4j and replaced it with slf4j and logback to address CVE-2021-44228.
