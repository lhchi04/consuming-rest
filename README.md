# consuming-rest

This repository contains the code for a client application that consumes RESTful API calls from the Quoters server. The client application is implemented using Spring Boot and Java.

## Build Instructions

To build and run the client application locally, follow these steps:

1. **Clone the Repository**: Clone this repository to your local machine using the following command:

    ```bash
    git clone https://github.com/lhchi04/consuming-rest.git
    ```

2. **Configure Quoters Service**: Ensure that the Quoters service is running locally or on a Kubernetes cluster.

   ```bash
   kubectl run quoters --image=javajon/quoters:1.0.0 --port=8080
   kubectl expose pod quoters --name=quoters
   kubectl get all -l run=quoters
    ```
   
3. **Start Port Forwarding**: If the Quoters service is running on Kubernetes, start port forwarding to localhost with the following command:

   ```bash
   kubectl port-forward service/quoters 8080:8080
   ```
   Leave this runnning, Ctrl-C to quit the port forwarding.

4. **Build the Application**: Use Gradle to build the application:

   ```bash
   ./gradlew build
   ```


## Dockerization

To deploy the client application on Kubernetes using Docker, follow these steps:

1. **Build Docker Image**: Build the Docker image using the provided Dockerfile:

   ```bash
   docker image build -t lhchi04/consumingrest .
   ```

2. **Push Docker Image**: Push the Docker image to a container registry:

   ```bash
   docker push lhchi04/consumingrest
   ```

3. **Run Deployment on Kubernetes**: Deploy the client application on Kubernetes using the following command:

   ```bash
   kubectl run consumingrest --image=lhchi04/consumingrest --env="quoters_base_url=http://quoters:8080"
   ```

4. **Verify Connection**: Verify that the client application is making the connection to the Quoters server by inspecting the logs of the pod:

   ```bash
   kubectl logs consumingrest
   ```