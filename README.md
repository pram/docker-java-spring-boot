# docker-java-spring-boot
Docker Java Spring Boot Test Application

## Installation Requirements - Windows

### WSL

Install WSL - [Offical Instructions](https://docs.microsoft.com/en-us/windows/wsl/install)
### Docker

Install Docker Desktop on Windows  [Official Instructions](https://docs.docker.com/desktop/windows/install/)
Install Docker engine on Linux  [Official Instructions](https://docs.docker.com/engine/install/)

### SDKMan

Open Linux Terminal

     sudo apt install unzip zip
     curl -s "https://get.sdkman.io" | bash
     source "$HOME/.sdkman/bin/sdkman-init.sh"

check that the installation is successful 

    sdk version

### Java

Install latest stable java

    sdk install java

### Spring Boot

Install Spring Boot

    sdk install springboot

## Creating the Application

Create a simple Spring web application

    spring init --dependencies=web demo-application

Test that the application works

    cd demo-application
    ./mvnw spring-boot:run

You should be able to navigate to the [app via a browser and see the default placeholder page](http://localhost:8080/)
