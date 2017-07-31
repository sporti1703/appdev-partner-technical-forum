<h2>Preparation to the workshop</h2>

##Download the VirtualBox VM

You need to download and import two VM image.

##Create Oracle Database Cloud Services using (unofficial) Cloud tool for Oracle Public Cloud

1. Locate the `environment.properties.emea2` or `environment.properties.us2` depending on the region where your domain was provisioned. Open the property file which fits for your account using *gedit* or *vi* text editor. Example:

		[oracle@localhost cloud-utils]$ gedit /u01/content/cloud-native-devops-workshop/cloud-utils/environment.properties.emea2 &

2. Prepare the property file which includes every information about your OPC access and desired services. The minimum set of properties to configure are the following:

   	**opc.identity.domain=YOUR\_OPC\_IDENTITY\_DOMAIN**<br>
   	**opc.username=YOUR\_OPC\_ACCOUNT\_USERNAME**<br>
   	**opc.password=YOUR\_OPC\_ACCOUNT\_PASSWORD**<br>
   	**ssh.passphrase=PASSPHRASE\_FOR\_PRIVATE\_KEY**<br>

3. Once the configuration of the properties ready save as `environment.properties`.
4. Generate key pair using this cloud tool. Make sure you have set (or changed the default) **ssh.passphrase=PASSPHRASE\_FOR\_PRIVATE\_KEY** and execute the following maven build:

		[oracle@localhost cloud-utils]$ mvn install -Dgoal=generate-ssh-keypair
		[INFO] Scanning for projects...
		[INFO]                                                                         
		[INFO] ------------------------------------------------------------------------
		[INFO] Building cloud-utils 1.0.0-SNAPSHOT
		[INFO] ------------------------------------------------------------------------
		[INFO] 
		[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ cloud-utils ---
		[INFO] Using 'UTF-8' encoding to copy filtered resources.
		[INFO] skip non existing resourceDirectory /u01/content/cloud-native-devops-workshop/cloud-utils/src/main/resources
		[INFO] 
		[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ cloud-utils ---
		[INFO] Nothing to compile - all classes are up to date
		[INFO] 
		[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ cloud-utils ---
		[INFO] Using 'UTF-8' encoding to copy filtered resources.
		[INFO] skip non existing resourceDirectory /u01/content/cloud-native-devops-workshop/cloud-utils/src/test/resources
		[INFO] 
		[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ cloud-utils ---
		[INFO] Nothing to compile - all classes are up to date
		[INFO] 
		[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ cloud-utils ---
		[INFO] Tests are skipped.
		[INFO] 
		[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ cloud-utils ---
		[INFO] 
		[INFO] --- maven-install-plugin:2.4:install (default-install) @ cloud-utils ---
		[INFO] Installing /u01/content/cloud-native-devops-workshop/cloud-utils/target/cloud-utils-1.0.0-SNAPSHOT.jar to /home/oracle/.m2/repository/com/oracle/wins/cloud/cloud-utils/1.0.0-SNAPSHOT/cloud-utils-1.0.0-SNAPSHOT.jar
		[INFO] Installing /u01/content/cloud-native-devops-workshop/cloud-utils/pom.xml to /home/oracle/.m2/repository/com/oracle/wins/cloud/cloud-utils/1.0.0-SNAPSHOT/cloud-utils-1.0.0-SNAPSHOT.pom
		[INFO] 
		[INFO] --- maven-antrun-plugin:1.8:run (first) @ cloud-utils ---
		[INFO] Executing tasks
		
		main:
		     [java] Read all properties from: file:/u01/content/cloud-native-devops-workshop/cloud-utils/environment.properties
		     [java] Selected goal: generate-ssh-keypair
		     [java] Generate SSH key pair ----------------------------------------
		     [java] WARNING! This will create new public and private key and overwrite the existing keypairs!
		     [java] The current keypairs will be copied with date postfix.
		     [java] Passphrase used from properties file (environment.properties): weblogic
		     [java] 
		     [java] Working dir: /u01/content/cloud-native-devops-workshop/cloud-utils/
		     [java] Private key backup: pk.openssh.20170726034414
		     [java] Public key backup: public.key.20170726034414
		     [java] Replacing public key in property file:/u01/content/cloud-native-devops-workshop/cloud-utils/environment.properties
		     [java] Keypair has been generated.
		[INFO] Executed tasks
		[INFO] ------------------------------------------------------------------------
		[INFO] BUILD SUCCESS
		[INFO] ------------------------------------------------------------------------
		[INFO] Total time: 4.511 s
		[INFO] Finished at: 2017-07-26T03:44:15-07:00
		[INFO] Final Memory: 14M/619M
		[INFO] ------------------------------------------------------------------------


	This will create a private key file (name: *pk.openssh*) and updates the **ssh.public.key** property with the generated value in the configuration file.<br> 
	This build also creates backup about the existing private key (pk.openssh -> pk.openssh.CURRENT\_TIME) and the public key from properties to a file (name: public.key.CURRENT\_TIME)<br>Please note if you want to use this generated private key for example Putty access you have to convert (puttygen) the private key to Putty's PPK format.

	Due to security reason it is important to change the ownersip of the file which contains the previously generated private key. To do so execute the following command in the `cloud-utils` directory.

		[oracle@localhost cloud-utils]$ chmod 400 pk.openssh

4. However the DBA credentials are provided, but obviously you can change that. Keep in mind during the lab you have to provide this password (too). Find the following property to know or modify the DBA password.

	**dbcs.dba.password=**

5. Create storage container using cloud util. If you would like to change the storage container name find the **opc.storage.container** property in your `environment.properties` file and  modify, otherwise please note the value which is needed later. Execute the next maven build using the *storage-create* goal.

		[oracle@localhost cloud-utils]$ mvn install -Dgoal=storage-create
		[INFO] Scanning for projects...
		[INFO]                                                                         
		[INFO] ------------------------------------------------------------------------
		[INFO] Building cloud-utils 1.0.0-SNAPSHOT
		[INFO] ------------------------------------------------------------------------
		[INFO] 
		[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ cloud-utils ---
		[INFO] Using 'UTF-8' encoding to copy filtered resources.
		[INFO] skip non existing resourceDirectory /u01/content/cloud-native-devops-workshop/cloud-utils/src/main/resources
		[INFO] 
		[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ cloud-utils ---
		[INFO] Nothing to compile - all classes are up to date
		[INFO] 
		[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ cloud-utils ---
		[INFO] Using 'UTF-8' encoding to copy filtered resources.
		[INFO] skip non existing resourceDirectory /u01/content/cloud-native-devops-workshop/cloud-utils/src/test/resources
		[INFO] 
		[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ cloud-utils ---
		[INFO] Nothing to compile - all classes are up to date
		[INFO] 
		[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ cloud-utils ---
		[INFO] Tests are skipped.
		[INFO] 
		[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ cloud-utils ---
		[INFO] 
		[INFO] --- maven-install-plugin:2.4:install (default-install) @ cloud-utils ---
		[INFO] Installing /u01/content/cloud-native-devops-workshop/cloud-utils/target/cloud-utils-1.0.0-SNAPSHOT.jar to /home/oracle/.m2/repository/com/oracle/wins/cloud/cloud-utils/1.0.0-SNAPSHOT/cloud-utils-1.0.0-SNAPSHOT.jar
		[INFO] Installing /u01/content/cloud-native-devops-workshop/cloud-utils/pom.xml to /home/oracle/.m2/repository/com/oracle/wins/cloud/cloud-utils/1.0.0-SNAPSHOT/cloud-utils-1.0.0-SNAPSHOT.pom
		[INFO] 
		[INFO] --- maven-antrun-plugin:1.8:run (first) @ cloud-utils ---
		[INFO] Executing tasks
		
		main:
		     [java] Read all properties from: file:/u01/content/cloud-native-devops-workshop/cloud-utils/environment.properties
		     [java] Selected goal: storage-create
		     [java] Create storage----------------------------------------
		     [java] Executing request GET https://appdev004.storage.oraclecloud.com/auth/v1.0 HTTP/1.1
		     [java] Response: HTTP/1.1 200 OK
		     [java] auth token:AUTH_tk0a28510466b082a35700297e93a180a0
		     [java] storage url:https://em2.storage.oraclecloud.com/v1/Storage-appdev004
		     [java] Output from Server .... 
		     [java] 
		     [java] 
		[INFO] Executed tasks
		[INFO] ------------------------------------------------------------------------
		[INFO] BUILD SUCCESS
		[INFO] ------------------------------------------------------------------------
		[INFO] Total time: 5.574 s
		[INFO] Finished at: 2017-07-26T03:46:20-07:00
		[INFO] Final Memory: 14M/619M
		[INFO] ------------------------------------------------------------------------

6. Here is the time to submit the Database Cloud Service provisioning request. This cloud util has also such function. Execute the maven build using the *dbcs-create* goal.

		[oracle@localhost cloud-utils]$ mvn install -Dgoal=dbcs-create
		[INFO] Scanning for projects...
		[INFO]                                                                         
		[INFO] ------------------------------------------------------------------------
		[INFO] Building cloud-utils 1.0.0-SNAPSHOT
		[INFO] ------------------------------------------------------------------------
		[INFO] 
		[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ cloud-utils ---
		[INFO] Using 'UTF-8' encoding to copy filtered resources.
		[INFO] skip non existing resourceDirectory /u01/content/cloud-native-devops-workshop/cloud-utils/src/main/resources
		[INFO] 
		[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ cloud-utils ---
		[INFO] Nothing to compile - all classes are up to date
		[INFO] 
		[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ cloud-utils ---
		[INFO] Using 'UTF-8' encoding to copy filtered resources.
		[INFO] skip non existing resourceDirectory /u01/content/cloud-native-devops-workshop/cloud-utils/src/test/resources
		[INFO] 
		[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ cloud-utils ---
		[INFO] Nothing to compile - all classes are up to date
		[INFO] 
		[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ cloud-utils ---
		[INFO] Tests are skipped.
		[INFO] 
		[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ cloud-utils ---
		[INFO] 
		[INFO] --- maven-install-plugin:2.4:install (default-install) @ cloud-utils ---
		[INFO] Installing /u01/content/cloud-native-devops-workshop/cloud-utils/target/cloud-utils-1.0.0-SNAPSHOT.jar to /home/oracle/.m2/repository/com/oracle/wins/cloud/cloud-utils/1.0.0-SNAPSHOT/cloud-utils-1.0.0-SNAPSHOT.jar
		[INFO] Installing /u01/content/cloud-native-devops-workshop/cloud-utils/pom.xml to /home/oracle/.m2/repository/com/oracle/wins/cloud/cloud-utils/1.0.0-SNAPSHOT/cloud-utils-1.0.0-SNAPSHOT.pom
		[INFO] 
		[INFO] --- maven-antrun-plugin:1.8:run (first) @ cloud-utils ---
		[INFO] Executing tasks
		
		main:
		     [java] Read all properties from: file:/u01/content/cloud-native-devops-workshop/cloud-utils/environment.properties
		     [java] Selected goal: dbcs-create
		     [java] DBCS create instance----------------------------------------
		     [java] https://dbcs.emea.oraclecloud.com/paas/service/dbcs/api/v1.1/instances/appdev004
		     [java] Response code: 202
		     [java] Response.... 
		     [java] 
		     [java] 
		[INFO] Executed tasks
		[INFO] ------------------------------------------------------------------------
		[INFO] BUILD SUCCESS
		[INFO] ------------------------------------------------------------------------
		[INFO] Total time: 9.767 s
		[INFO] Finished at: 2017-07-26T03:46:44-07:00
		[INFO] Final Memory: 14M/619M
		[INFO] ------------------------------------------------------------------------
		
	Check the response code which has to be: 202. Database Cloud Service provisioning requires >15 minutes.

7. Check the Database Cloud Service status. Use the cloud util to check the status of the service. Now specify the *dbcs-get-instance-details* goal.

		[oracle@localhost cloud-utils]$ mvn install -Dgoal=dbcs-get-instance-details
		[INFO] Scanning for projects...
		[INFO]                                                                         
		[INFO] ------------------------------------------------------------------------
		[INFO] Building cloud-utils 1.0.0-SNAPSHOT
		[INFO] ------------------------------------------------------------------------
		[INFO] 
		[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ cloud-utils ---
		[INFO] Using 'UTF-8' encoding to copy filtered resources.
		[INFO] skip non existing resourceDirectory /u01/content/cloud-native-devops-workshop/cloud-utils/src/main/resources
		[INFO] 
		[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ cloud-utils ---
		[INFO] Nothing to compile - all classes are up to date
		[INFO] 
		[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ cloud-utils ---
		[INFO] Using 'UTF-8' encoding to copy filtered resources.
		[INFO] skip non existing resourceDirectory /u01/content/cloud-native-devops-workshop/cloud-utils/src/test/resources
		[INFO] 
		[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ cloud-utils ---
		[INFO] Nothing to compile - all classes are up to date
		[INFO] 
		[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ cloud-utils ---
		[INFO] Tests are skipped.
		[INFO] 
		[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ cloud-utils ---
		[INFO] 
		[INFO] --- maven-install-plugin:2.4:install (default-install) @ cloud-utils ---
		[INFO] Installing /u01/content/cloud-native-devops-workshop/cloud-utils/target/cloud-utils-1.0.0-SNAPSHOT.jar to /home/oracle/.m2/repository/com/oracle/wins/cloud/cloud-utils/1.0.0-SNAPSHOT/cloud-utils-1.0.0-SNAPSHOT.jar
		[INFO] Installing /u01/content/cloud-native-devops-workshop/cloud-utils/pom.xml to /home/oracle/.m2/repository/com/oracle/wins/cloud/cloud-utils/1.0.0-SNAPSHOT/cloud-utils-1.0.0-SNAPSHOT.pom
		[INFO] 
		[INFO] --- maven-antrun-plugin:1.8:run (first) @ cloud-utils ---
		[INFO] Executing tasks
		
		main:
		     [java] Read all properties from: file:/u01/content/cloud-native-devops-workshop/cloud-utils/environment.properties
		     [java] Selected goal: dbcs-get-instance-details
		     [java] DBCS get specific instance details----------------------------------------
		     [java] Executing request GET http://jcs.emea.oraclecloud.com/paas/service/dbcs/api/v1.1/instances/appdev004/techcoDB HTTP/1.1 Auth: {<any realm>@jcs.emea.oraclecloud.com:443=[principal: peter.nagy@oracle.com]}
		     [java] Response: HTTP/1.1 200 OK
		     [java] Output from Server .... 
		     [java] 
		     [java] {
		     [java]   "service_name": "techcoDB",
		     [java]   "version": "12.1.0.2",
		     [java]   "status": "Running",
		     [java]   "description": "techcoDB db instance through REST",
		     [java]   "identity_domain": "appdev004",
		     [java]   "creation_time": "2017-07-26T10:46:44.092+0000",
		     [java]   "last_modified_time": "2017-07-26T10:46:44.050+0000",
		     [java]   "created_by": "peter.nagy@oracle.com",
		     [java]   "sm_plugin_version": "17.3.1-547",
		     [java]   "tools_version": "17.3.1-547",
		     [java]   "backup_supported_version": "16.2.3",
		     [java]   "service_uri": "https:\/\/jcs.emea.oraclecloud.com:443\/paas\/service\/dbcs\/api\/v1.1\/instances\/appdev004\/winsdemo",
		     [java]   "database_id": "0",
		     [java]   "num_nodes": 1,
		     [java]   "level": "PAAS",
		     [java]   "edition": "EE",
		     [java]   "shape": "oc3",
		     [java]   "use_high_performance_storage": false,
		     [java]   "subscriptionType": "MONTHLY",
		     [java]   "creation_job_id": "6681451",
		     [java]   "num_ip_reservations": 1,
		     [java]   "backup_destination": "BOTH",
		     [java]   "cloud_storage_container": "Storage-appdev004\/wins_container",
		     [java]   "cloud_storage_user": "peter.nagy@oracle.com",
		     [java]   "cloud_storage_pass": "XPi53yhOmOhor5q",
		     [java]   "failover_database": false,
		     [java]   "rac_database": false,
		     [java]   "current_version": "12.1.0.2.170418",
		     [java]   "sid": "ORCL",
		     [java]   "pdbName": "PDB1",
		     [java]   "demoPdb": "yes",
		     [java]   "listenerPort": 1521,
		     [java]   "timezone": "UTC",
		     [java]   "em_url": "https:\/\/140.86.1.205:5500\/em",
		     [java]   "connect_descriptor": "winsdemo:1521\/PDB1.appdev004.oraclecloud.internal",
		     [java]   "connect_descriptor_with_public_ip": "140.86.1.205:1521\/PDB1.appdev004.oraclecloud.internal",
		     [java]   "apex_url": "https:\/\/140.86.1.205\/apex\/pdb1\/",
		     [java]   "glassfish_url": "",
		     [java]   "dbaasmonitor_url": "https:\/\/140.86.1.205\/dbaas_monitor",
		     [java]   "charset": "AL32UTF8",
		     [java]   "ncharset": "AL16UTF16",
		     [java]   "is_clone": false,
		     [java]   "clone_supported_version": "16.3.1",
		     [java]   "service_associations": [],
		     [java]   "compute_site_name": "EM002_Z12",
		     [java]   "jaas_instances_using_service": ""
		     [java] }
		     [java] 
		[INFO] Executed tasks
		[INFO] ------------------------------------------------------------------------
		[INFO] BUILD SUCCESS
		[INFO] ------------------------------------------------------------------------
		[INFO] Total time: 8.366 s
		[INFO] Finished at: 2017-07-28T01:41:01-07:00
		[INFO] Final Memory: 14M/619M
		[INFO] ------------------------------------------------------------------------

	The output above shows detailed information about a running Database Cloud Service instance. Find the `"status": "Running"` property to check the service readiness.



