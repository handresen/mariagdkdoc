---
title: Maria GDK service installation
keywords: service, installation, addin, configuration, virtualization
tags: [service]
sidebar: core_setup_sidebar
permalink: core_setup_service_installation.html
summary: Maria GDK documentation is primarily intended for development teams using the Maria GDK platform and for members of the Maria GDK development team. 
---

Maria Geo Developement Kit consists of a set of services and client side assemblies that can be used to develop a customized GIS application. The following resources cover the configuration and installation of the services and their relationships.

- [Installing service addins](./core_setup_service_installation_addins.html) (Maria GDK 2.0 and newer)
- [Configuring service addins](./core_setup_service_configuration.html)
<br/>
- Installing services (Maria GDK 1.5 and older)

## Virtualization of Maria GDK

This article defines the supported virtual platforms for Maria GDK.

### Running services in virtual hardware

Running the services on virtual hardware is supported for all Windows platforms supported by the Maria GDK using VMWare or Hyper-V.

### Running client using Remote Desktop

This scenario is more complicated due to graphics limitation in some configurations. The following table shows which tested configurations works and which does not.

 | Configuration                           | Console window | Remote desktop | Comment                                                                                                                                                                                                              | 
 | -------------                           | -------------- | -------------- | -------                                                                                                                                                                                                              | 
 | Windows Server 2008 R2 VMware           |                |                | VMWare machine must be configured with 3D support. [VMWare help](https///​pubs.vmware.com/​vsphere-55/​index.jsp?​topic=%2Fcom.vmware.vsphere.vm_admin.doc%2FGUID-E03ED27D-E469-4115-80E1-435125D6168B.html) | 
 | Windows Server 2008 R2 Hyper-V          | No             | No             |                                                                                                                                                                                                                      | 
 | Windows Server 2012 R2 VMware           | Yes            | Yes            |                                                                                                                                                                                                                      | 
 | Windows Server 2012 R2 Hyper-V          | Yes            | Yes            |                                                                                                                                                                                                                      | 
 | Windows Server 2016 Hyper-V             | Yes            | Yes            |                                                                                                                                                                                                                      | 
 | Windows 7 Service Pack 1 64 bit Hyper-V | No             | No             |                                                                                                                                                                                                                      | 
 | Windows 7 Service Pack 1 32 bit Hyper-V | No             | No             |                                                                                                                                                                                                                      | 

