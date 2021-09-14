# Problem Statement
We have needs for a safe place to keep encryption keys, certificates and hash values in the boot process. The tool needs to have some kind of non volatile memory and it should be easy for us to communicate with.

# General Information:
TPM follows an ISO standard created by TCG to ensure a standardized way for platform owners to use hardware security to validate software operations.
For a brief overview of different TPM categories [Brief Overview pdf](https://trustedcomputinggroup.org/wp-content/uploads/2019_TCG_TPM2_BriefOverview_DR02web.pdf) provides the most basic information

# TPM Architecture
A TPM is some kind of tamper-resistant techonology that can perform cryptographic operations (including key generation) and protect small amounts of sensitive information, such as passwords and cryptographic keys.

It is built as shown in the picture below with a non volatile storage to keep our certificates and encryption keys, a set of secure platform registers for hash values, and a interface enabling us to operate it. ![TPM Architecture](/images/TPM_Architecture.jpg)
(source: https://trustedcomputinggroup.org/resource/trusted-platform-module-tpm-summary/)

# TCG Software Stack (TSS) - For communication and operation:
To ensure a standardized way of operating a TPM, TCG created the following TPM 2.0 ISO standard which describes an operations library TPM creators must implement: Publicly Available Standards 
 
The ISO standard is derived from the specifications that makes up the TCG software stack 2.0

First off, we have the [Tab (TPM access broker) and resource manager (RM) specification](https://trustedcomputinggroup.org/wp-content/uploads/TSS_2p0_TAB_ResourceManager_v1p0_r18_04082019_pub.pdf). It describes the physical capabilities and defines what kind of work the Tab and RM can do for the upper TSS layers.

Next is the [ marshaling and unmarshaling API (MUAPI) specification](https://trustedcomputinggroup.org/wp-content/uploads/TCG_TSS_Marshaling_Unmarshaling_API_v1p0_r07_pub.pdf). It makes up a layer which is responsible for translating between the language specific representation (C) and the network data stream.

[The command transmission interface (TCTI)](https://trustedcomputinggroup.org/wp-content/uploads/TCG_TSS_TCTI_v1p0_r18_pub.pdf) is responsible for creating the transmission context between the higher and lower layers of the stack. As another way to understand this layer of the TSS a parralel can be made to OSIM layer 3, 4 and 5 because the TCTI has to make sure the right reciever gets the message, that the message is delivered properly and that the message has been handled propperly.

To interact with the lower levels of the TSS, TCG created the [TSS API (SAPI) specification](https://trustedcomputinggroup.org/wp-content/uploads/TSS_SAPI_v1p1_r29_pub_20190806.pdf) as an abstraction layer so that users don't have to write the raw instructions for the Tab and thereby make it easier for users to control TPM operations. 

Because the SAPI has a high complexity TCG made a specification for an even further abstraction with the [Enhanced system API (ESAPI)](https://trustedcomputinggroup.org/wp-content/uploads/TSS_ESAPI_v1p0_r08_pub.pdf).
 
To increase the ease of use, TGC made a high level API as an abstraction layer on top of ESAPI that is called the [feature API (FAPI)](https://trustedcomputinggroup.org/wp-content/uploads/TSS_FAPI_v0p94_r09_pub.pdf). This layer does some of the workload for the programmer by making default assumptions about objects and design choices. This makes it so that 80% of TPM use cases can be programmed by interacting with the FAPI. For the last 20% that has to do custom configurations, FAPI would have to be supplied by the other API layers. 


# Solutions
- Intel TXT
- AMD FTPM
- Self configured TPM stack

## Intel TXT: 
Intel® Trusted Execution Technology (Intel® TXT) provides an extension to Intel's chipset and therefore enables a hard coupling between the hardware used, and the system.
The result is a hardened chain of trust from secure boot to TPM measurements by utilizing the access to the Intel's chipset. [(Intel® TXT Enabling Guide)](https://www.intel.com/content/www/us/en/architecture-and-technology/trusted-execution-technology/txt-enabling-guide.html)  
### Pros:
Intel® TXT creates a secured environment for each application to run, which protects an application against software attacks from outside the environment [Intel® Trusted Execution Technology overview](https://www.intel.com/content/www/us/en/support/articles/000025873/technologies.html).
That should also make us able to create operation policies based on security clearances. In short we can defines which processor is allowed to perform tasks with higher security clearance.
### Cons:
Because the stack uses the tool [TrouSerS](https://sourceforge.net/p/trousers/trousers/ci/master/tree/) it is unclear if this stack supports the 64 bit architecture and TPM 2.0.

## AMD FTPM
AMD has firmware TPM (FTPM) since the Ryzen 2500 CPU that only require us to enable it in the BIOS.
### Pros:
This solution provides us with an easy TPM since it just comes with the CPU.
### Cons:
Unlike Intel TXT AMD does not provide extra utility in the form of chip extensions that can be used for enhanced security features. 
The solution only gives us a firmware TPM which is less secure than a discrete or integrated TPM.

## Self configured TPM stack
This option gives the platform owner, the possibility to create and support the chain of trust without being hardware restricted as much. In this instance we would be able to use the Secure Boot and apply a discrete TPM with a TSS that gives us everything we need to operate a TPM.
### Pros:
It enables us to use whatever tools we feel fits the best and in turn makes us able to support TPM v2.
### Cons:
No chip extensions that gives us enhanced security features such as secure environment for each application.