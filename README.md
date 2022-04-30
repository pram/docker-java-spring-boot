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

Update the default code with an endpoint.

Open the file `demo-application\src\main\java\com\example\demoapplication\DemoApplication.java` and replace the contents

````
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class DemoApplication {
  @RequestMapping("/")
  public String home() {
     return "Hello Everybody!";
  }
  public static void main(String[] args) {
    SpringApplication.run(DemoApplication.class, args);
  }
}
````

Restart the app and you should see content on the default homepage.

Copy the files `Dockerfile` and `docker-compose.yml` into the root level of the demo application. Then run the following command

    docker compose build --no-cache demo-application

You can start the application using the command

    docker compose up -d

Shut down the container with

    docker compose down