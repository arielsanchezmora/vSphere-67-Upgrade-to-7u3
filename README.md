# vSphere-67-Upgrade-to-7u3

Tips, tricks, gotchas and important dates collected by a friendly VMware TAM

## vSphere 7 ushers in a new era

vSphere 7 has lots of new capabilities. This link is the best to see them all listed and find technical information on each  
https://blogs.vmware.com/vsphere/vsphere-7   

There is also a blog post highlighting the new vSphere 7u3 features:  
https://core.vmware.com/blog/vsphere-7-update-3-whats-new  

vCenter 7.x is only available as an appliance:  
https://blogs.vmware.com/vsphere/2017/08/farewell-vcenter-server-windows.html  

vSphere 7u3c should be the minimum version to upgrade to, as it solves Log4J vulnerabilities  
https://blogs.vmware.com/vsphere/2022/01/announcing-availability-of-vsphere-7-update-3c.html  


### TAM customer webinars  

TAM Customer Webinar detailing v7 storage changes  
https://www.youtube.com/watch?v=CXHxsrL5KKg&list=PLXw1EF8ZER1i0hKUm2OaUp7xTYdp504cA&index=1  

TAM Customer Webinar detailing vSphere 7  
https://www.youtube.com/watch?v=OVN9nNtjJwI  

TAM Lab vSphere 7.x Planning and Upgrade  
https://www.youtube.com/watch?v=yTmR7Rucz5Q&t=185s   


### Known issues in 7u3
There are some known issues with vSphere 7u3. For example, if you have VMs with more than 8TB of RAM, as is customary with in-memory databases like SAP HANA, you may need to follow the workaround listed here https://blogs.vmware.com/apps/2022/02/sap_hana_vsphere70u3c_and_cooper_lake_8s_support.html  

A complete list of known issues is included and updated in the release notes and in this KB “Important list of Knowledge base articles identified for vSphere 7.0 U3c release”  
https://kb.vmware.com/s/article/87327   


## Important dates:
As of 7/3/22, ESXi 6.5 and 6.7 show their End of General Support (EOGS) on 10/15/22 (vCenter has the same dates):  
https://lifecycle.vmware.com/  

![lifecycle screenshot vsphere 6.x](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image1.jpg)  
 
What does End of General Support mean?  
https://www.vmware.com/support/policies.html#lifecyclepolicies  

![general support explanation](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image2.jpg)  


## General documentation on upgrade process

vSphere 7 upgrade documentation  
https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.esxi.upgrade.doc/GUID-65B5B313-3DBB-4490-82D2-A225446F4C99.html  

This link centralizes upgrade documentation and introduces the vSphere Upgrade Assessment tool  
https://www.vmware.com/products/vsphere/upgrade-center.html  

vSphere 7 Upgrade Best Practices KB  
https://kb.vmware.com/s/article/78205  

Important information before upgrading to vSphere 7 KB  
https://kb.vmware.com/s/article/78487  

TAM blog post on upgrading to vSphere 7  
https://blogs.vmware.com/customer-experience-and-success/2022/03/your-4-step-vsphere-7-upgrade-guide-tips-from-a-vmware-tam.html  


## Version upgrade recommendations

Because of the fix to the log4j vulnerability, it is recommended all customers upgrade to at least 7u3c:  
https://blogs.vmware.com/vsphere/2022/01/announcing-availability-of-vsphere-7-update-3c.html  

Please note, if your 6.7 or 6.5 build is newer (released on a date after) than 7u3c, you could run into issues with the upgrade and have to use a newer version of 7u3. For example, in the tables below, vCenter 6.7u3q was released in Feb 2022, which means you shouldn’t try to update to 7u3c, released in January 2022 – always try to upgrade to a newer (by date) version of vSphere 7, which gives you the advantage of bug fixes.  

![vcenter 7  release tables](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image3.jpg)  
![vcenter 6.7 release tables](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image4.jpg) 

vCenter build numbers and release dates are listed in https://kb.vmware.com/s/article/2143838  
ESXi build numbers and release dates are listed in https://kb.vmware.com/s/article/2143832   

vSphere 7u3 had a rocky start, with all prior versions to 7u3c start being recalled. You can find historical information in these two links:  
https://blogs.vmware.com/vsphere/2021/11/important-information-on-esxi-7-update-3.html  
https://blogs.vmware.com/vsphere/2022/01/announcing-availability-of-vsphere-7-update-3c.html  

It is **very important** to read the release notes for your version:  

vSphere ESXi 7 Update 3c Release Notes  
https://docs.vmware.com/en/VMware-vSphere/7.0/rn/vsphere-esxi-70u3c-release-notes.html  

vCenter Server 7 Update 3c Release Notes  
https://docs.vmware.com/en/VMware-vSphere/7.0/rn/vsphere-vcenter-server-70u3c-release-notes.html  

This link was already mentioned in the first section, but I'm repeating it below for completeness: vSphere 7 Update 3c – List of Known Issues and Workarounds  
https://kb.vmware.com/s/article/87327  


## Highlight from ESXi 7u3 release notes

When 7u3 was announced, booting from SD storage was discouraged. Many customers were already using SD cards for boot and this was very inconvenient, especially for a minor upgrade. The changed code has been rolled back, but the recommendation for higher quality boot devices in the future remains  

https://blogs.vmware.com/vsphere/2021/09/esxi-7-boot-media-consideration-vmware-technical-guidance.html  
https://kb.vmware.com/s/article/85685  

- The /locker partition might be corrupted when the partition is stored on a USB or SD device  
- Due to the I/O sensitivity of USB and SD devices, the VMFS-L locker partition on such devices that stores VMware Tools and core dump files might get corrupted.  

This issue is resolved in this release. By default, ESXi loads the locker packages to the RAM disk during boot.  


## Highlights from vCenter Server 7u3 release notes

A driver problem that stopped the upgrade was identified. Please note the part I have put in bold in the following statement. This document assumes upgrades from 6.7 mainly but includes this information just in case.  

Due to the recent name change in the Intel i40en driver to i40enu and back to i40en, ESXi hosts in some environments later than ESXi 7.0 Update 2a might have both driver versions, which results in several issues, all resolved in vSphere 7.0 Update 3c. **Affected ESXi versions are 7.0 Update 3a, 7.0 Update 3, 7.0 Update 2d, and 7.0 Update 2c**. VMware provides a _vSphere_upgrade_assessment.py_ script that you can use to identify any ESXi hosts that require remediation before you start a vCenter Server upgrade. To download the script and for more information, see VMware knowledge base article 87258, and the instructional video on the vCenter Server 7.0 Update 3c (7.0.3.00300) upgrade precheck.  https://www.youtube.com/watch?v=xSA58Xzf8Wg  

When you start the update or upgrade of your vCenter Server system, an upgrade precheck runs a scan to detect if ESXi hosts of versions potentially affected by the issues around the Intel driver name change exist in your vCenter Server inventory. If the precheck identifies such ESXi hosts, a detailed scan runs to provide a list of all affected hosts, specifying file locations where you can find the list, and providing guidance how to proceed.


# Upgrade planning


## Minimizing unknown risk

History teaches that no amount of planning will consider all use cases. Thus, it’s best to introduce strategies meant at minimizing risk.  

It’s important to develop company-specific documentation for the upgrade process. This will allow many colleagues to collaborate on upgrades and lessons learned to be communicated.  

The following are considerations that should be addressed before deciding on a date for the upgrades.


## Backups

The first step is to have reliable backups. Confirm and test your backups are in good working order before upgrading.  

Types of backups to check and verify before attempting upgrades:  

- vCenter  
- Distributed virtual switches  
- ESXi config (especially important as v7 uses a new format and wipes the previous installation) https://kb.vmware.com/s/article/2042141  

Also check for re-installation media for the current versions of vSphere, as well as any vendor drivers/firmware that would be needed for a rollback situation.  


## Target environments with less risk first

One way to minimize risk is to have several independent environments where upgrades can be tested in order from least impactful up to production. For example:  

Start with a true test environment, that can be down at any time, and can be rebuilt several times with different drivers, firmware and hardware configurations. The biggest concern is that it may not have the same exact integrations and hardware as production. The biggest advantage is practice and speed.  

Next should be any development environment that is not tied to production workloads. Hopefully this environment has some integrations, such as backup or monitoring, and hardware is in line with production.  

A point of conflict could be environments that are in linked mode. vCenter upgrades require all vCenters in linked mode to be upgraded at the same time. These upgrades must be planned very carefully.  

Production environment upgrades should consider the level of impact, hardware and number of integrations. A VDI environment, for example, has more considerations than a server environment. If possible, develop or simulate these integrations in test or dev.  


## dVS Upgrades carry some risk

Distributed virtual switch upgrades should be done off hours as they can cause a network “blip”. Please read and understand the following KB  
https://kb.vmware.com/s/article/52621  


## Use GSS and TAM services to assist prior to the upgrade

Always open a proactive GSS ticket prior to upgrades. Present your plan to GSS and ask them to verify it. You may require 5 business days or more, depending on many factors, so open with time. Of course, your TAM will also verify to the best of their ability.


## Hardware considerations

Hardware compatibility should be checked for all types of hardware currently deployed in the environment using the VMware Compatibility Guide  

https://www.vmware.com/resources/compatibility/search.php  

Several filters can be applied. Sometimes the same server model can have different family CPUs, so this should also be checked (the following screenshot is not comprehensive, but has selected the 7u3 ESXi version and a manufacturer to see all supported servers by that vendor)  

![VMware Compatibility Guide](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image5.jpg)  

Develop and **document** a hardware plan, both for software and hardware firmware and drivers versions. It’s important to verify the hardware vendor’s website for their recommended ESXi installation ISO image, which should include all drivers needed, and related firmware upgrade CD. They typically include release notes or “recipe” documentation to make sure you are aligned; the VMware hardware compatibility web page can also be used to verify IO device driver and firmware combinations.  

![software, firmware and driver version documentation](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image6.jpg)  

It’s important to get familiarized with the replacement for VUM (vSphere Upgrade Manager) called vSphere Lifecycle Management. It is similar but introduces new features. These two links are a great start:  

https://core.vmware.com/resource/introducing-vsphere-lifecycle-management-vlcm  

https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere-lifecycle-manager.doc/GUID-74295A37-E8BB-4EB9-BFBA-47B78F0C570D.html  

The best practice is to use your test environments to familiarize yourself with the new interface.  


## Licensing

An upgrade from v6 to v7 will require updating licenses. Document old license keys and their location, and upgrade license keys as you upgrade environments. You will need licenses for all upgraded components, including individual hosts and vSAN.  


## Security hardening

A complete review of hardening steps has to be done with this upgrade. The steps that were used to harden v6.7 will be different in v7, although many will be similar. Always refer to the official hardening guide link to find the best recommendation:  

https://www.vmware.com/security/hardening-guides.html  

https://blogs.vmware.com/vsphere/2020/10/announcing-the-vsphere-security-configuration-guide-7.html  


## Follow recommended order for all VMware component upgrades

There is a specific upgrade order when several VMware products are installed together. Familiarize yourself with this KB and follow the order in your upgrade plan document.  

https://kb.vmware.com/s/article/78221  


## Checking vCenter integration dependencies

vSphere integrates with many solutions. Most of the integrations are done at the vCenter level. These include both VMware and 3rd party solutions.  

Develop a list of all the environments and what software integrations exist. Common ones include backups, monitoring, automation, hardware and storage plugins. You can use this table as an example of the documentation that you should create:  

![vCenter integration documentation](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image7.jpg)  

Use the product interoperability matrix to check applicable versions  

https://interopmatrix.vmware.com/Interoperability  

For example, SRM is required to be at version 8.5 to be compatible with vSphere 7u3:  

![SRM and vCenter version dependencies](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image8.jpg)  

Develop this list _before_ starting upgrades. It will bring clarity of what systems share integrations (for example, the same backup platform may service both dev and production environments) and will save many avoidable headaches.


## VCSA free space

You will get an option to migrate the most essential data, or to also include tasks, events and performance metrics. You can compare the size of the data with the available free space in the source vCenter. When the data is larger than available free space, you will be asked for a different location than / . Typically, you will find the VUM partition has large free space (using the command _df -h_) and can provide that, check this [blog post by Luciano Patrao](https://www.provirtualzone.com/how-to-add-extra-space-to-vcenter-for-the-upgrade/) or consult with GSS.

![VCSA upgrade data option](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image10.jpg)


## vSphere Diagnostic Tool 

A tool that VMware GSS has made available to the public in fling form is able to catch many problems in vCenter environments before they become an issue mid-upgrade. It is a good idea you run this and share the output in your proactive GSS ticket, as it may show some tasks that need to be performed before you attempt the upgrade.  

https://flings.vmware.com/vsphere-diagnostic-tool  


## Further Reading

I want to link to several of [Lucho DeLorenzi](https://twitter.com/lgdelorenzi)'s blog posts, which are very useful especially when you want to get your hands dirty and check the health of your vCenters yourself. This should be covered during your proactive ticket with VMware GSS, but just in case :)

https://luchodelorenzi.com/2021/04/05/how-do-i-get-to-vsphere-7-0-without-dying-in-the-process/  
_video is in Spanish, courtesy of the Argentina VMUG_  

https://luchodelorenzi.com/2020/04/10/pre-upgrade-considerations-in-multi-vcenter-environments/  

https://luchodelorenzi.com/2020/05/28/proactively-checking-and-replacing-sts-certificate-on-vsphere-6-x-7-x/  

# Upgrade execution


This section is a good start, but you will have to adjust and expand to your environment and conditions.  Place the VCSA VM in a known host and stop automatic movement of VMs by putting DRS in manual mode. You may also clone the VCSA VM as an extra backup, or create a snapshot, but be aware that linked mode vCenters need a cold snapshot of all vCenters linked, and this is something you should be doing in coordination with GSS.

## Upgrade day preparation checklist

1.	Finish upgrade documentation plan  
2.	Gather temporary IP address for vCenter(s)  
3.	Gather needed software and hardware ISOs  
4.	Test ESXi host remote access credentials  
5.	Set a longer maintenance window than needed  
6.	Have proactive GSS ticket handy  

## Upgrade order list

An actual execution order should be in your upgrade documentation plan. The below is an example (which is not meant to be comprehensive):  

1. Backup the environment  
2. Upgrade vCenter server  
3. Install vCenter Plugins  
4. Upgrade ESXi Hosts  
5. Upgrade the vSphere Distributed Switches  
6. Upgrade VMware Tools  
7. Upgrade VMware Virtual Hardware  
8. Upgrade vSAN on-disk Format to 7.0 Compatibility  
9. Upgrade Cisco UCS Infrastructure  
10. Upgrade the Cisco UCS C-Series Servers with HUU  
11. Upgrade storage array infrastructure  


## dVS v7 new functionality

v7 includes new functionalities for the distributed virtual switch, which can be reviewed here:

https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.networking.doc/GUID-330A0689-574A-4589-9462-14CA03F3F2F4.html  

![vSphere 7 new functionality](https://raw.githubusercontent.com/arielsanchezmora/vSphere-67-Upgrade-to-7u3/main/images/vSphere67-Upgrade-7u3-image9.jpg)




# Post upgrade considerations

Check that all integrations, users, monitoring and automations are working. Perform tests. As you work out any issues in each environment, document the fixes as lessons learned for the next environment.  

If you created snapshots, don't hold them for too long. Snapshots can stress the storage system, and they are only useful for a few hours; if you must, 2 days should be more than enough of a time window to have taken a good backup.  

Great resource for vCenter performance:  

https://blogs.vmware.com/performance/2021/09/extreme-performance-video-blog-series.html  

vCenter now uses vCLS (vSphere Clustering Service):

https://core.vmware.com/resource/introduction-vsphere-clustering-service-vcls  

https://kb.vmware.com/s/article/80472  

Enjoy vCenter 7!
