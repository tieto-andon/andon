# Continuous Delivery boilerplate called Andon
### Solution paper about makeing Container Driven Continuous Delivery/Deployment (CD) easy as Code

## Problem

In modern software development fully automated continuous delivery is a de facto to release your software as soon as possible with good quality. In reality this is not always so easy when teams face human, technological and financial constraints in their projects. Here is a list of common issues.

* No competence to setup up the Continuous Delivery pipe (tools) <br>
* No competence to implement efficient Test Automation (human resources) <br>
* No competence on Test Environment orchestration (human resources) <br>
* Technological dependencies in development and testing (software architecture) <br>
* No resources for extra CAPEX and OPEX for infra <br>
* No time to setup everything manually (time) <br>
* License and configuration problems when using e.g. 3rd party testing software (agreement issues) <br>
* Continuous Delivery configuration management is not hardened <br>

These problems sums up to be a quite hefty project overhead, especially when working with fast deliveries.

## Solution

Andon can be thought like a Continuous Delivery boilerplate, where Andon gives the team the proven way to implement Continuous Delivery and its components as easily as possible. Andon gives and guides the team to use best tools (open source and licenced) and practices in a self-service manner. [Here](https://github.com/tieto-devops-demos) you can find an Ordering system (microservice) application, which is being deployed using Andon configuration and tooling. Andon utilizes the YML files [andon.yml](https://github.com/tieto-devops-demos/microservice-demo-order/blob/master/andon.yml) added to the software projects root. 
```yml
general:
  whitelist: true
  pipeline_base_name: Order
#  dependency:
#   - Pipeline X [Stage X]
#   - Pipeline Y [Stage Y]

#pre_stage:
#  go_cd_agent_props:
#    - java:8
#    - maven
#  custom_cmd:
#    - true
#    - true

compile:
  go_cd_agent_props:
    - java:8
    - maven
  sonarqube: true
  stages:
    - stage: Compile
      jobs:
        - job: Maven_and_Docker
          tasks:
            - task: mvn clean -Djava.security.egd=file:/dev/./urandom install
              build_path: .
              run_if_conditions: passed
            - task: sudo docker build -f Dockerfile -t order:$GO_PIPELINE_COUNTER .
              build_path: .
              run_if_conditions: passed

integration_testing:
  compose_file_path: /src/test/resources
  compose_file_name: docker-compose.yml

  service_virtualization:
    mari_file_path: src/test/resources/service-virtualization/VirtualServices
    mari_file_names:
      - catalog.mari

  tests:
    go_cd_agent_props:
      - java:8
      - maven
    test_services:
      service_virtualization: true
      robot_framework: true
      cucumber: false
    stages:
      - stage: IntegrationTests
        jobs:
          - job: Robot
            selenium_grid: true
            browser_stack: false
            tasks:
              - task: mvn -P robot -Drobot.url=http://order:8080 -Drobot.remote_url=http://hub:4444/wd/hub -Drobot.browser=chrome install
                build_path: .
                run_if_conditions: passed

  selenium:
    version: "2.53.1"
    browsers:
    - name: chrome
      amount: 1
    - name: firefox
      amount: 1

post_stage:
  go_cd_agent_props:
    - java:8
    - maven
  custom_cmd:
#      - sudo docker login -u eekamak -p $passu
      - sudo docker tag order:$GO_PIPELINE_COUNTER order:latest
```
After this project has been imported by using Andon UI. Andon Engine will configure GO CD to be able run the following tasks:
1. Builds java source code <br>
2. Executes unit tests <br>
3. Run static code analysis <br>
4. Builds docker image for integration and acceptance testing <br>
5. Creates test environments on demand with local Selenium Grid <br>
6. Starts service virtualization for tests <br>
7. Executes REST API test made my by Robot Framework <br>


Every project will have their own andon.yml and by combining all of them you are able to create complex pipelines with quite ease. Here is an example pipeline of the Ordering Microservice application (Image 1):
![Product Deployment master](https://github.com/tieto-andon/andon/blob/master/pics/prod_deployment_master.PNG)
Image 1: Pipeline configured with Andon to GoCD
Here is a simplified architecture picture (Image 2) where the main concepts are described.
![Andon architecture](https://github.com/tieto-andon/andon/blob/master/pics/andon_overview.png)
Image 2: Andon architecture

Name of Andon is derived from [Kaizen manufacturing principles](https://en.wikipedia.org/wiki/Andon_\(manufacturing\)) to bring same idea to the software development lifecycle management. The point is to pinpoint problems in CD pipe as soon as possible in a visible way. Fail fast and left shift are the ways to go. 

## Conclusion
In order to get left shift and fail fast methods in use to your projects you need repeatable and automated solutions that are supporting the Software Lifecycle Management. If your projects scales, use prototyping or use branching then tooling needs to scale too. Andon has the best of breed from open source and licensed world, delivered with convenience. 

Andon promotes and support following tools and practices to be utilized in your software development project:
* Docker technologies for running the application in test bed and on your own env
* Acceptance Test Automation with Robot Framework (Acceptance Test Driven Development)
* Security testing with OWASP ZAP and HP Fortify
* Service Virtualization for technical dependency removal
* Ansible for Infrastructure as Code approach
* GO Continuous Delivery as CD Engine
* Selenium Grid for scalable frontend testing
* CD configuration as Code with YML format

Thanks for reading!


There is always room to improve! Check this [protip](https://www.ted.com/talks/terry_moore_how_to_tie_your_shoes?language=en) out! :)
