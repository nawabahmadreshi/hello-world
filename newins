This page provides a summarized listing of the essential steps for planning, installing, and configuring your Sunbird deployment in a live environment. 

## Pre-requisites

Before you install Sunbird into the production environment, examine your  environment and gather data to ensure an optimal installation experience. 

Review the following to determine that the environment has the necessary resources and compliant target systems to successfully install and run Sunbird.

- System requirements
- System variables
- API keys
- Installation checklist
- Server Setup

### System Requirements

To install Sunbird, the servers should have the following minimum system requirements:

   - Operating System: Ubuntu 16.04 LTS
   - RAM: 7GB
   - CPU: 2 cores, >2 GHz
   - root access (should be able to sudo)

### Relevant Variables

To make the deployment scenario simple and clear, use the following variables:

  - **implementation-name** : This variable refers to the name of sunbird implementation. For example; Sunbird-Demo

  - **environment-name** : This variable refers to the name of the environment where you deploy Sunbird. For example; Development, Test , Staging, Production, etc. 

### API Keys

API keys are supposed to approve a user or a device with authorization for access.For getting the API keys , you need to send an Email to :[info@sunbird.org](mailto:info@sunbird.org). 

### Installation Checklist

Before you start the installation process make sure you have successfully 

- Review and check whether the system requirements are met. 
- Check whether or not you  have the API keys.
- Decide the installation owner and the installation directory  
- Gather information about the database, if you are installing the database server while installing Sunbird
- Set up or verify the product licensing requirements for the live environment
- Verify the required network requirements and that ports are available for Sunbird components to communicate

### Provisioning servers

Follow either an automated or manual process to provision the servers. For setup in a non-production environment, use only the manual process. Use the automated process if you are setting up Sunbird and are not sure of setting up the infrastructure correctly, or if you plan to roll out your implementation to serious users.

#### Automation

The set of scripts on the [Automation for Azur](http://will%20link%20to%20azure%20automation%20page)[e](http://will%20link%20to%20azure%20automation%20page) page create the network and servers needed to run Sunbird. The default configuration procedure provisions for three servers, with the minimum specifications mentioned in the System requirements section.
Known-how of  Azure: VNet, Resource Group, etc. is beneficial but not mandatory. 

**Note:** The automation walk-throughs provided (PART 1) , (PART2), shows you the automated process. You can use them to understand the commands to be used and assist you in the process for provisioning the servers.

### Others

Not automated as of now but you are free to contribute. Follow our contribution guidelines to contribute.

**Automation walkthrough**

[Part 1]([https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-1.gif){:target="_blank"}](https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-1.gif){:target=\"_blank\"})

[Part 2]([https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-2.gif){:target="_blank"}](https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-2.gif){:target=\"_blank\"})

**Note:** The default automation process creates three servers because it separates the application and the administration server.

#### Manual

The manual procedure commissions two servers. The first server serves as the DB(Database) server and the second serves as the application server & administration server.

The automated provisioning setup sets up: 
* Azure Virtual Network (aka VPC in AWS) 
* Creates multiple subnets (one for swarm master, one for swarm slave machines and DB servers), 
* Creates master servers, a replication set of slaves (so that you can add or subtract slave nodes easily),load balancers for master and slaves, 
* Opens up ports for communication between servers, creates a DB server, sets up FQDNs and runs the Docker Swarm.

The manual setup is cumbersome and exhaustive and thus not recommended. Also, it is not supported currently. It is recommended to use the automation scripts instead. However, you are always welcome to contribute code for making the deployment process simple and easy.

For some reasons if  you wish to set up manually, the main requirement is to have Docker Swarm installed and working (multi node cluster), servers available to install the DB and ports open for communication.

### Set up Databases

You can either use existing databases, create them manually or run the automation scripts provided to create them. Sunbird uses  the following databases:

   - Cassandra
   - Postgres
   - Mongo
   - Elasticsearch
#### Preparation to Set Up Databases

Run the following steps from your local machine:

		#1 Configuring the database servers

1. SSH into the `db-server`. If you have not edited the default configuration, then the name of the DB VM should be `db-1`. Automated setup does not expose the DB to the Internet, so for SSH to access the DB, it’s important to set SSH to `vm-1` (check out `master FQDN` above) with `ssh -A` (key forwarding) and then SSH to `db-1`.

		#2 Cloning the repository

2. Clone the sunbird-devops repo using `git clone [https://github.com/project-sunbird/sunbird-devops.git`](https://github.com/project-sunbird/sunbird-devops.git`) in the console.

		#3 Creating environment configuration

3. Run `./sunbird-devops/deploy/generate-config.sh <implementation-name> <environment-name>`. 

E.g. `./sunbird-devops/deploy/generate-config.sh mysb production deploy`. 

This creates `mysb-devops` directory with *incomplete* configurations. 

The missing configuration needs to be done afterwards.

		#4 Modifying configurations

4. Modify all the configurations under `# DB CONFIGURATION` block in `<implementation-name>-devops/ansible/inventories/<environment-name>/group_vars/<environment-name>`

The estimated  run time for  preparation to Set up Databases is 15-30mins and another 30 mins to complete complete the process.

##### Creating Databases using  automation

Estimated elapsed time is  10-15 mins (in an environment with fast internet).

Following is a set of scripts which installs the Databases into the `db-server` and copies over the `master` data.

- Run `cd sunbird-devops/deploy`

- Run `sudo ./install-dbs.sh <implementation-name>-devops/ansible/inventories/<environment-name>`. 

**Automation Walkthrough**

[Part 4]([https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-4.gif){:target="_blank"}](https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-4.gif){:target=\"_blank\"})

##### Create Databases Manually 

To create the databases manually, refer to the corresponding database user guides from their respective websites.

#### Initialize Databases

Initialize the databases after they are installed using the following the procedure:

Run `sudo ./init-dbs.sh <implementation-name>-devops/ansible/inventories/<environment-name>`

**Note:** The automation walk-through provided (PART 4), shows you the creation of databases and the initialization process.

### Set up Application and Core services

For Setting up the Application and the core services :

 Follow the process 

1. SSH into `admin-server`. 

**Note :** If you have used automated scripts used here, then this server would be `vm-1`.

2. Clone the sunbird-devops repo using `git clone [https://github.com/project-sunbird/sunbird-devops.git`](https://github.com/project-sunbird/sunbird-devops.git`) .

3. Copy over the configuration directory from the database server(`<implementation-name>-devops`) to this machine.

4. Modify all the configurations under `# APPLICATION CONFIGURATION` block.

5. The automated setup also creates a proxy server, it requires a SSL certificate. 

6. Details of the certificates must be added in the configuration. 

**Note:** If you don't have SSL certificates, to get started you can generate and use Self signed certificates .The walkthrough for generating one can be found here:

 [self-signed certificates]([https://en.wikipedia.org/wiki/Self-signed_certificate){:target="_blank"}](https://en.wikipedia.org/wiki/Self-signed_certificate){:target=\"_blank\"}).

7. Run `cd sunbird-devops/deploy` in console 

#8 Executing the following command will install the dependencies.

8. Run `sudo ./install-deps.sh`. 

#9 Executing the following command will onboard various APIs and consumer groups.

9. Run `sudo ./deploy-apis.sh <implementation-name>-devops/ansible/inventories/<environment-name>`. 

****Note:**** The following steps are necessary only when the application is being deployed for the first time and could be skipped for subsequent deploys.

10. deploy-apis.sh script will print a JWT token that needs to be updated in the application configuration. 

11. To find the token search the script output , and look for the string. "JWT token for player is :", 

12. Copy the corresponding token,

Example output of token is below:

<pre>

  changed: [localhost] => {"changed": true, "cmd": "python /tmp/kong-api-scripts/kong_consumers.py

  /tmp/kong_consumers.json ....... "

  JWT token for player is :

      "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJlMzU3YWZlOTRmMjA0YjQxODZjNzNmYzQyMTZmZDExZSJ9.L1nIxwur1a6xVmoJZT7Yc0Ywzlo4v-pBVmrdWhJaZro", 

  "Updating rate_limit for consumer player for API cr......"]}

 </pre>

Now as token has been copied , Proceed with the following steps:

13.  Update `sunbird_api_auth_token` in your configuration with the above copied token.

14. Update `sunbird_ekstep_api_key` in your configuration with the API token obtained from ekstep portal

The following script creates the keycloak username,groupname and keycloak service on virtual machine. Keycloak is deployed on vm. 

15. RUN `./provision-keycloak.sh <implementation-name> devops/ansible/inventories/<environment-name>`.

16. Update variables in the config  ` <implementation-name>-devops/ansible/inventories/<environment-name>/group_vars/<environment-name>`.

   ```

    keycloak_password: (which admin initial password)

    keycloak_theme_path: ex- path/to/the/nile/themes. 

   ```

Sample themes directory of sunbird can be  found  [here](https://github.com/project-sunbird/sunbird-devops/tree/master/ansible/artifacts){:target="_blank"}

Run `sudo ./deploy-keycloak-vm.sh <implementation-name>-devops/ansible/inventories/<environment-name>`.

#### Update the following configuration files 

<pre>

Login to the keycloak admin console, goto the clients->admin-cli->Installation->Select json format

sunbird_sso_client_id: # Eg: admin-cli

sunbird_sso_username: # keycloak user name

sunbird_sso_password: # keycloak user password

Login to the keycloak admin console, goto the clients->portal->Installation->Select json format

keycloak_realm:  # Eg: sunbird

sunbird_keycloak_client_id: # Eg: portal

Login to the keycloak admin console, goto the clients->trampoline->Installation->Select json format

sunbird_trampoline_client_id:  # Eg: trampoline

sunbird_trampoline_secret:     # Eg: HJKDHJEHbdggh23737

</pre>

**Note: **You can customize the homepage, logo, and icon for your portal at this point. For details, refer to Customizing Sunbird page.

### Deploying Sunbird services

The Sunbird services are  categorized into :

* Sunbird Core Services

* Sunbird proxy services

#1. To deploy the Sunbird core services ,execute the following command:

Run `sudo ./deploy-core.sh <implementation-name>-devops/ansible/inventories/<environment-name>`. 

#2. To deploy  the Sunbird proxy services , execute the following command:

Run `sudo ./deploy-proxy.sh <implementation-name>-devops/ansible/inventories/<environment-name>`. 

**Note:** The automation walk-through provided (PART 5)(PART6)&(PART7), shows you the process for  deployment for Sunbird services.

** Automation Walkthrough**

 [Part 5]([https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-5.gif){:target="_blank"}](https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-5.gif){:target=\"_blank\"})

[Part 6]([https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-6.gif){:target="_blank"}](https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-6.gif){:target=\"_blank\"})

[Part 7](https://sunbirdpublic.blob.core.windows.net/installation/demo/demo-8.gif){:target="_blank"}

### Check  the Installation

To check if Sunbird is properly installed:

Browse Sunbird Portal  by accessing https://{proxy_server_name}/ (publicly accessible URL, it could be the load balancer URL or the actual domain name for production).

### Generate key and secrets for mobile app

This section details the sequence of steps required if you plan to release your own mobile app using the Sunbird mobile app codebase.

* Execute & run `sudo ./deploy-apis.sh <implementation-name>-devops/ansible/inventories/<environment-name>`

* In console output of script, copy the JWT token printed for `mobile_admin` user

* Run it.

	<pre>

		curl -X POST \

		  (sunbird-base-url)/api/api-manager/v1/consumer/mobile_app/credential/register \

		  -H 'authorization: Bearer (mobile_admin_jwt_token)' \

		  -H 'content-type: application/json' \

		  -d '{

		  "request": {

			"key": "(implementation-name)-mobile-app-(version-number)"

		  }'

		}

	</pre>

The result output will be:

	<pre>

		{"result":{"key":"(implementation-name)-mobile-app-(version-number)","secret":"(secret)"}}

	</pre>

*  Use the value of "key" and "secret" from the response above for `MOBILE_APP_KEY` and `MOBILE_APP_SECRET` configuration in mobile app.

## Upgrade to a new version of Sunbird

Sunbird updates are released on a regular basis. For upgrading to the latest version of Sunbird .

To update/redeploy sunbird please follow these steps:

1. Update the Sunbird image versions to latest gold version (e.g. `PLAYER_VERSION`).

2. Update the configuration as per Sunbird release notes.

3. Run `cd sunbird-devops/deploy`

#4.Executing the following command will onboard various APIs and consumer groups.

4. Run `sudo ./deploy-apis.sh /ansible/inventoriesRun `

#5.Executing the following command will setup all the sunbird core services.

5. sudo ./deploy-core.sh /ansible/inventories/`. 

#6.Executing the following command will setup sunbird proxy services.

6. Run `sudo ./deploy-proxy.sh /ansible/inventories/`. 

