![](common/ptf.header.png)

## Prerequisites

#### Virtualbox VM images

This workshop requires the [cando.v2.0.ova](https://drive.google.com/open?id=0B0MXC4qaECO6RHBWMEttdW9fOVk) virtualbox image to be downloaded and imported.

#### Oracle Public Cloud PaaS  account

The workshop is intended to work with an Oracle PaaS trial account. Get the following account details ready to complete the tutorial and replace to your values when it is required:

+ Oracle Cloud account **username** and **password**
+ Oracle Cloud **identity domain**
+ **Data center/region**

IMPORTANT: Before you start to use your new Oracle Public Cloud services make sure that the replication policy has been set for your account. Otherwise you can not create storage container which is necessary for most of the services. See [Selecting a Replication Policy for Oracle Storage Cloud Service](https://docs.oracle.com/cloud/latest/storagecs_common/CSSTO/GUID-5D53C11F-3D9E-43E4-8D1D-DDBB95DEC715.htm).

#### Create Database Cloud Service instance

It is recommended to provision Database Cloud Service instance in advance because it requires >20 minutes to complete. Use the [cando.v2.0.ova](https://drive.google.com/open?id=0B0MXC4qaECO6RHBWMEttdW9fOVk) image and follow the [instructions](preparation.md).

----

#### Moving workload to the cloud ####

+ [Lab 1: Migrate Weblogic 10.3.6 (on premise) Application to Java Cloud Service with App2Cloud tool](app-2-cloud/README.md)
+ [Lab 2: Scaling Down Nodes in a Java Cloud Service Instance Using the Command Line Interface](jcs-scale-down-psm/README.md)
+ [Lab 3: Scaling WLS Nodes in a Java Cloud Service Instance](jcs-scale-up-ui/README.md)

#### DevOps and Cloud Native Microservices

+ [Lab 3: Agile Project Management](microservices/CloudNative100.md)
+ [Lab 4: Continuous Delivery of Java Microservices](microservices/CloudNative200.md)
+ [Lab 5: Cloud Native Rapid JavaScript Development with Node.js](microservices/CloudNative300.md)
+ [Lab 6: Cloud Native Developer Cloud Service Administration](microservices/CloudNative400.md)

#### Deploy complex cloud environment using Oracle Cloud Stack Manager ###
+ [Lab 7: FixItFast Cloud Native Application which includes multiple Oracle Application Container Cloud Services (Cache, Java, NodeJS)](stack/stack.cache.md)

#### Container based application development lifecycle using Wercker and Container Cloud Service ####

+ [Lab 8: Build Node.js-MongoDB container packaged application using Wercker and deploy to Oracle Container Cloud Service as the Stack of services](nodejs-mongodb-stack/README.md)

---

### Contributing

Pull Requests are currently not being accepted. 

### [License](LICENSE.md)
