![Andon Logo](https://github.com/tieto-andon/andon/blob/master/pics/Andon_logo.PNG)

# Andon Release Notes
Fixes will be implemented only to the newest version.

## Version v18.3.1:
* GOCD server and agents version v18.3.0
* Docker login added in andon yml under deploy_env

## Version v18.3.0:
* GOCD server and agents version v18.3.0

## Version v18.2.0:
* GOCD server and agents version v18.2.0

## Version v18.1.0:
* GOCD server and agents version v18.1.0

## Version v17.12.0:
* GOCD server and agents version v17.12.0

## Version v17.11.0:
* GOCD server and agents version v17.11.0

## Version v17.10.0:
* GOCD server and agents version v17.10.0

## Version 0.17.2:

* GOCD server and agents version 17.2.0
* In andon.yml copose_file_name changed to compose_file_names having a list of names
* Android Java 8 agent taken into use
* In andon.yml intgration_testing and acceptance_testing compined to testing
* In CentOS fixed problems when used sudo with maven command, sudo not used anymore. The sudo is used only with docker commands, take this into account also if docker commands used in andon.yml pre or post stage.
* In andon.yml it is possible to define artifacts for a job

## Version 0.4.6:

* Version 0.4.6:
* GOCD server and agents version 17.1.0
* 17.1.0 only Java 8 is supported
* Browser Stack support added

## Version 0.4.5:

* GOCD server and agents version 16.12.0
* In ubuntu maven commands cannot be run with sudo. Files under target have root:root acces and cannot be deleted when cleaning is done. In 0.4.4 CentOs didn’t work without sudo.
* About menu item removed
* Shell script for building agents created
* The commands phase in andon.yml changed to stages phase, type removed. In compile, integration test and acceptance test same stages phase used.
* Docker 1.13.0 and Docker Compose 1.10.0 is mandatory
* Andon Engine Update added into Help menu
* Bug fix: in integration and acceptance test phases only the latest task was taken into account, now all tasks are taken into account.
* Bug fix: build_path and run_if_conditions were on wrong level, moved from job to task.
* Bug fix: checking default agent was working only when pipeline was imported. Now it is checked every 10 minutes.

## Version 0.4.4.1:

* GOCD server and agents version 16.10.0
* CentOS 7 support
* Otherwise same as 0.4.4
* Reason: in ServiceNow robot tests hangs up wit GOCD 16.11.0
* Added python 2.7 and robotframework libraries into java 8 maven 3 agent, due report not always created

## Version 0.4.4:

* GOCD server version 16.11.0
* GOCD agents updated to 16.11.0
* Dynamical job naming
* New UI look&feel
* Custom tabs visible only if flag in andon.yml is set
* Job names can be defined in andon.yml
* Agent scaling up and down improved
* Cucumber report custom tab
* Version info on main page.
* Agent version update possible from UI.
* Selenium grid version is possible to define in andon.yml in integration and acceptance tests. If not defined latest tag is pulled, if defined the version is used in docker pull..
* Agent properties default value removed. Any value in andon.yml is accepted for agent resource. Pipeline will hang if no agent found with specified resource.
* Added console message if pipeline update fails, shows json returned from GO CD
* NPM4 agent support added
* Clojure leiningen agent support with java 8
* Python 2.7 and 3.5 agent support
* Reimplement agent recognising from andon.yml resources
* Added GO_CD_VERSION in andon compose file, agent starting compose files use that
* Copyright 2016 Tieto added
* Bug fix: adding hubport environment value in selenium grin compose file
* Bug fix: In acceptance testing phase service virtualization agents were started when they were not configured to be used.
* Bug fix: In reading andon.yml in acceptance testing the command type was not set properly.
* Bug fix: reading pre and post stage was depending on general phase, without general they were not read
* Bug fix: docker registry ip changed to host name
* Bug fix: selenium grid pull is done before up, always using newest version.
* Bug fix: configure pipeline phase can be executed only in agent andon engine provides, not in any agent. GO CD default agent has set a resource and configure pipeline uses that resource.
* Bug fix: pre and post stage agent properties were not correctly used when services were checked to be running. Fixed now and pre and post stage agents are correctly started if they are not running.
* Bug fix: Integration test phase used wrong resources in pipeline creation
* Bug fix: production phase used wrong resources in creating pre and post stages


## Version 0.4.3:

* GOCD server version 16.10.0
* New andon.yml content
* About tab showing version information
* Settings tab content changed in UI
* When Andon engine starts is sets ANSIBLE_HOST_KEY_CHECKING environment variable to default value (False) if it does not exists. If values exists they are not changed
* Private key uploading in UI Settings tab
* Production phase in andon.yml has same structure as compile phase
* Agent scaling in UI Setting tab
* UI is showing environment variables when Setting tab is opened



## Version 0.4.2:

* OWASP ZAP integration to RF test cases
* Environment variable support from Andon UI
    * SonarQube authentication
* GOCD server version 16.9.0
    * ln -s /var/lib/go-server/go-server.log /var/log/go-server/go-server.log
    * Log file has changed location
* GOCD agents updated to 16.9.0 version
* Pipeline material uses whitelist if working directory is defined
* UI changed to use tabs: Andon home, Docker login and Settings
* New tab Settings: Can create environment variables in UI and those will be used in pipelines. Secret variables are also possible. In UI you can fetch all created environment variables.
* In Docker login tab all docker users can be fetched
* All commands use now “/bin/bash -c”
* The commands running shell scripts with sudo added -E argument
* Docker login tab removed
* Settings tab removing environment variable is possible
* Project name set as SCM material name
* When Andon engine starts is sets SONARUSER and SONARPASSWORD environment variables to default values (admin/admin) if they do not exists. If values exists they are not changed
* OWASP ZAP support added
* In andon.yml general part can be defined whitelist: [true|false]. If not in general part whitelist default value is true. If whitelist is false nothing is filtered, e.g. it is not blacklist


## Version 0.4.1:

* New andon.yml feature:
    * Docker Image name and tag support
    * Pipeline name addition
    * SV recording MAR file updated
    * Support for non maven build in build command
    * Go CD agent properties support added for missing phases
* Docker 1.11.2 and Docker Compose 1.8.0 is mandatory
* Surefire artifact added only when maven command is used
* In andon.yml files GO agent properties can be defined in integration test, acceptance test and production phases
* When Andon is checking are required GO agents running it checks the agent status. If status is missing (server has no connection to agent) agents with required resources are started again
* Docker login in UI is done inside agent which is doing docker push
* Production phase removed from acceptance test phase. Production is now own project
* In andon.yml added post_stage phase with custom command list in compile and unit test, integration test, acceptance test and production phases. If commands are defined post_stage stage is added into pipeline
* When docker login is done user data is stored into an encrypted file is user data is not yet there. When pipeline configuration is run users are red from encrypted file and for every user docker login is executed in agent containers.
* Bug fix: when project name is defined in andon.yml pom.xml was not correctly read

## Version 0.4.0:

* Support for DevOps Space deployment
* SVN support added
* New demo application: https://github.com/Kalle80/microservice. 
    * Forked from https://github.com/ewolff/microservice
* Java 8 support for GoCD Agents 
* GoCD Server and Agent upgraded to 16.6
* Andon UI changes

## Version 0.3.X and older

* Maven 3 and Java 7 support for agents
* Support for Docker Compose version 2
* Selenium Grid integration to parallel test execution
* Parallel and Serial test execution
* andon.yml reformatting
* GoCD 12 support
* Andon UI for project import
* Old demo application: https://github.com/Kalle80/bookapp (not supported from 0.4.0 onwards)
