<!--
  Focused Devs - 2024
-->
# Nexus Repository Composer Plug In

# Table Of Contents
* [Developing](#developing)
   * [Requirements](#requirements)
   * [Building](#building)
* [The Fine Print](#the-fine-print)

## Developing

### Requirements

* [Apache Maven 3.3.3+](https://maven.apache.org/install.html)
* Network access to https://repository.sonatype.org/content/groups/sonatype-public-grid

### Building

#### Build with Docker

    docker buildx build --platform linux/arm64 -t nexus-repository-composer .

#### Run as a Docker container

    docker run -d -p 8081:8081 --name nexus-repository-composer nexus-repository-composer 

The application will now be available from your browser at http://localhost:8081

* As of Nexus Repository Manager Version 3.17, the default admin password is randomly generated.
  If running in a Docker container, you will need to view the generated password file 
  (/nexus-data/admin.password) in order to login to Nexus. The command below will open a bash shell 
  in the container named `nexus-repository-composer`:

      docker exec -it nexus-repository-composer $ cat /nexus-data/admin.password 
      
  Once logged into the application UI as `admin` using the generated password, you should also 
  turn on "Enable anonymous access" when prompted by the setup wizard.

#### Set up in composer

In order to get composer to query first in your nexus repository, the following
configurations are recommended:

* In case you have not set a secure connection for your repo yet (these is not recommended for production!)

      composer config secure-http false
* Tell composer to go to your localhost nexus repo
      
      composer config repo.focus composer http://localhost:8081/repository/focus-group
* And not to go to the Packagist repo
  
      composer config repo.focus composer http://localhost:8081/repository/focus-group

Now by installing or requiring packages, those should be able to pull them from the nexus repo.
Your composer.json shoudl have a block like this one:

    "repositories": {
        "packagist": false,
        "focus": {
            "type": "composer",
            "url": "http://localhost:8081/repository/focus-group"
        }
    }

## The Fine Print

This is **NOT SUPPORTED** by Focus Devs, and is a contribution of ours
to the open source community

Remember:

* Use this contribution at the risk tolerance that you have
* Do NOT file Focus Devs support tickets related to Composer support in regard to this plugin
* DO file issues here on GitHub, so that the community can pitch in