---
slug: '/'
sidebar_label: 'Cegid Orli Connector'
---

# Cegid Orli Connector

**Latest Version is 22.1**

ORLI Commercial Management was entirely conceived to be a set of tools to aid decision-making for all business management areas.

Note that that Connector version 22.0.0 supports Cegid Orli wsdl v2 and 21.0.0 supports Cegid Orli wsdl v1. 

The connector implementation consists of a Windows batch program that is executed by the Windows Agent. The job definitions are entered as Windows jobs using the Cegid ORLI job subtype. 
When the job is scheduled by OpCon, the definitions are passed as arguments to the Cegid ORLI Connector. 

The Cegid ORLI Connector supports the following job types requestFiles, requestLog, requestStatus, requestTechnicalData and executeRequest which can used be used to communicate with the Cegid application environment.

- requestFiles job type can be used to retrieve the list of files available after the execution of the request (executionRequest).
- requestLog job type can be used to retrieve the log information after the execution of the request (executionRequest).
- requestStatus job type can be used to determine the status of an execution request (executionRequest).
- requestTechnicalData job type can be used to retrieve technical data about an execution request (executionRequest). This is usually only used to obtain additional information when an error occurs.
- executeRequest job type can be used to start a job within the Cegid ORLI application. After submitting an executionRequest, the status of the execution is monitored until completion (either successful or failure) and the appropriate job information and logs are extracted and added to the OpCon job output. 

The job definitions are passed to the Cegid ORLI Connector as arguments. The connector uses the BatchManage.wsdl definition to define the web services end points. The job definition information received from OpCon is then mapped to the appropriate structures and the web service is called. 

