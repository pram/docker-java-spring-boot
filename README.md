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

## Working with the Application

You can now work with the Application in an IDE

### VS Code

Create a `launch.json` file in the `.vscode` folder if one doesn't exist already.

Add the following snippet

```
{
    "type": "java",
    "name": "Attach to Remote Program",
    "request": "attach",
    "hostName": "localhost",
    "port": 8000
}
```

You can now select the option called "Attach to Remote Program" in the debug dropdown and attach to the instance running within the container. You will also be able to click the “Hot Module Swap” button (the lightning bolt icon) and VS Code will update the running application. [Refresh the link to view the results](http://localhost:8080/)

### Intellij

Double click on the file `docker-compose.yml` and click on the double pointer next to the word `services`. This will run `docker compose up`.

Now create a Remote Debug configuration and set the port to 8000. It should show that the remote session is connected.

You can now apply breakpoints and dynamically modify code using the Build -> Recomppile... menu item or pressing CTRL-SHIFT-F9

## Kubernetes

Install Kubernetes or activate in the Docker Desktop settings within Windows

Activate Kubernetes Desktop. From the command line run the following command. Get the latest version by checking the [Web UI Dashboard Page](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). As of the time of writing the command is

    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml

You should now see multiple apps in the "Container / Apps" section of the Docker desktop application.

Now patch the deployment to disable the login screen (Don't do this is prod)

    kubectl edit deployment kubernetes-dashboard -n kubernetes-dashboard

Text editor will open. Look for the section `/spec/template/spec/containers/0/args/`. Approx line 41 in v2.5.0. Add the following line

    - --enable-skip-login

The entire section should now look like the following

```
spec:
    containers:
    - args:
    - --auto-generate-certificates
    - --namespace=kubernetes-dashboard
    - --enable-skip-login
```

Exit the text editor and the updated setting should be applied.

Now run the command to login to the dashboard

    kubectl proxy

You should see the following message

    Starting to serve on 127.0.0.1:8001

You can now [login to the dashboard](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/)

Stop that running and install the Metrics Server.

Run the following command

    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

You will again need to edit the deployment to allow for insecure connections (again don't do this on prod)

    kubectl edit deployment metrics-server -n kube-system

Add the following at approx line 45

    - --kubelet-insecure-tls

The Section should now look like

```
 spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        - --kubelet-insecure-tls
```