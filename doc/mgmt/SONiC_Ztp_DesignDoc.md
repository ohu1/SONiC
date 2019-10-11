# Zero Touch Provisioning (ZTP)
ZTP support through sonic-management framework
# High Level Design Document
#### Rev 0.1

# Table of Contents
  * [List of Tables](#list-of-tables)
  * [Revision](#revision)
  * [About This Manual](#about-this-manual)
  * [Scope](#scope)
  * [Definition/Abbreviation](#definitionabbreviation)

# List of Tables
[Table 1: Abbreviations](#table-1-abbreviations)

# Revision
| Rev  |    Date    |      Author       | Change Description |
| :--: | :--------: | :---------------: | ------------------ |
| 0.1  | 10/10/2019 | Arunsundar Kannan | Initial version    |

# About this Manual
This document provides general information about the ZTP support inside SONiC management framework.
# Scope
Covers Northbound interface for the ZTP feature, as well as Unit Test cases.

# Definition/Abbreviation

### Table 1: Abbreviations
| **Term** | **Meaning**             |
| -------- | ----------------------- |
| ZTP      | Zero Touch Provisioning |

# 1 Feature Overview

Provide management framework capabilities to handle:

- Show status of ztp service
- Enable ztp service
- Disable ztp service
- Run ztp service


## 1.1 Requirements

### 1.1.1 Functional Requirements

Provide management framework support to existing SONiC capabilities with respect to ZTP

### 1.1.2 Configuration and Management Requirements
- CLI style configuration and show commands
- REST API support
- gNMI Support

### 1.1.3 Scalability Requirements
N/A
### 1.1.4 Warm Boot Requirements

## 1.2 Design Overview
### 1.2.1 Basic Approach
The feature will use the traditional translib/transformer approach with an addition of a service running in the host which does all the "actual work". The service will use the existing sonic ztp scripts in the host.

### 1.2.2 Container
Management Container

### 1.2.3 SAI Overview
N/A

# 2 Functionality
## 2.1 Target Deployment Use Cases

**show ztp-status**

This command displays the current ZTP configuration of the switch. It also displays detailed information about current state of a ZTP session. It displays information related to all configuration sections as defined in the switch provisioning information discovered in a particular ZTP session.

**ztp enable**

The *ztp enable* command is used to administratively enable ZTP. When ZTP feature is included as a build option, ZTP service is configured to be enabled by default. This command is used to re-enable ZTP after it has been disabled by user. It is to be noted that this command will only modify the ZTP configuration file and does not perform any other actions.

**ztp disable**

The *ztp disable* command is used to stop and disable the ZTP service. If the ZTP service is in progress, it is aborted and ZTP status is set to disable in configuration file. The ZTP service does not run if it is disabled even after reboot or if startup configuration file is not present. User will have to use *ztp enable* for it to enable it administratively again.

**ztp run**

Use this command to manually restart a new ZTP session. This command deletes the existing */etc/sonic/config_db.json* file and stats ZTP service. It also erases the previous ZTP session data. The ZTP configuration is loaded on to the switch and ZTP discovery is performed. This command is useful to restart ZTP after it has failed or has been disabled by user.

Note: The above text is taken from the sonic-ztp readme (https://github.com/Azure/SONiC/blob/master/doc/ztp/ztp.md)

## 2.2 Functional Description

Upon receiving a request, the app module forwards the request to the service running at the host, which calls the backend implementation to do all ztp related tasks.

# 3 Design
## 3.1 Overview
TBD

## 3.2 DB Changes

N/A
### 3.2.1 CONFIG DB

N/A

### 3.2.2 APP DB

N/A

### 3.2.3 STATE DB

TBD. 

### 3.2.4 ASIC DB

N/A

### 3.2.5 COUNTER DB

N/A

## 3.3 Switch State Service Design
### 3.3.1 Orchestration Agent

N/A

### 3.3.2 Other Process
N/A

## 3.4 SyncD
N/A

## 3.5 SAI
N/A

## 3.6 User Interface
### 3.6.1 Data Models
TBD (Working on a custom Sonic yang).

### 3.6.2 CLI
#### 3.6.2.1 Configuration Commands

**ztp enable**

```
sonic# ztp enable
Success
```

**ztp disable**

```
sonic# ztp disable
Success
```

**ztp run**

```
sonic# ztp run
Success
```

#### 3.6.2.2 Show Commands

**sonic_installer list**

```
sonic# show ztp-status
ZTP Admin Mode : True
ZTP Service    : Inactive
ZTP Status     : SUCCESS
ZTP Source     : dhcp-opt67 (eth0)
Runtime        : 05m 31s
Timestamp      : 2019-09-11 19:12:24 UTC

ZTP Service is not running

01-configdb-json: SUCCESS
02-connectivity-check: SUCCESS
```

Use the verbose option to display more detailed information.

```
sonic# show ztp-status --verbose
========================================
ZTP
========================================
ZTP Admin Mode : True
ZTP Service    : Inactive
ZTP Status     : SUCCESS
ZTP Source     : dhcp-opt67 (eth0)
Runtime        : 05m 31s
Timestamp      : 2019-09-11 19:12:16 UTC
ZTP JSON Version : 1.0

ZTP Service is not running

----------------------------------------
01-configdb-json
----------------------------------------
Status          : SUCCESS
Runtime         : 02m 48s
Timestamp       : 2019-09-11 19:11:55 UTC
Exit Code       : 0
Ignore Result   : False

----------------------------------------
02-connectivity-check
----------------------------------------
Status          : SUCCESS
Runtime         : 04s
Timestamp       : 2019-09-11 19:12:16 UTC
Exit Code       : 0
Ignore Result   : False
```

Note: The above text is taken from the sonic-ztp readme (https://github.com/Azure/SONiC/blob/master/doc/ztp/ztp.md)

#### 3.6.2.3 Debug Commands

N/A

#### 3.6.2.4 IS-CLI Compliance

N/A

### 3.6.3 REST API Support

**TODO**
TBD (Working on a custom Sonic yang).

# 4 Flow Diagrams
N/A

# 5 Error Handling

TBD

# 6 Serviceability and Debug

TBD

# 7 Warm Boot Support

TBD

# 8 Scalability
N/A

# 9 Unit Test
List unit test cases added for this feature including warm boot.

# 10 Internal Design Information

