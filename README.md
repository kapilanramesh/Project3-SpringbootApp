**Spring Boot WAR app deployed via Jenkins and Dockerized Tomcat**

---

## 🚀 Spring Boot WAR App CI/CD with Jenkins, Docker, and Tomcat

This project demonstrates a complete CI/CD pipeline for a **Spring Boot Servlet-based WAR application** using:

* **Maven** to build the WAR
* **Dockerized Tomcat** to host the app
* **Jenkins** for CI/CD automation
* **GitHub** as source control

---

### 📁 Project Structure

```
springboot-war-app/
├── Dockerfile
├── Jenkinsfile
├── pom.xml
└── src/
    ├── main/
    │   ├── java/
    │   │   └── com/example/
    │   │       └── SpringbootWarAppApplication.java
    │   ├── resources/
    │   │   └── application.properties
    │   └── webapp/
    │       └── WEB-INF/
    │           └── web.xml
```

---

### ⚙️ Technologies Used

* Java 11
* Spring Boot (WAR packaging)
* Apache Tomcat 9
* Maven
* Docker
* Jenkins
* GitHub

---

### 📦 Build Instructions

1. Clone the repo:

   ```bash
   git clone https://github.com/YOUR_USERNAME/springboot-war-app.git
   cd springboot-war-app
   ```

2. Build the WAR file:

   ```bash
   mvn clean package
   ```

3. Dockerfile contents:

   ```Dockerfile
   FROM tomcat:9.0-jdk11
   COPY target/*.war /usr/local/tomcat/webapps/ROOT.war
   CMD ["catalina.sh", "run"]
   ```

4. Build Docker image:

   ```bash
   docker build -t springboot-war-app .
   ```

5. Run Docker container:

   ```bash
   docker run -d --name springboot-war-container -p 8080:8080 springboot-war-app
   ```

---

### ⚙️ Jenkins Setup

1. Make sure Jenkins user is added to Docker group:

   ```bash
   sudo usermod -aG docker jenkins
   sudo systemctl restart jenkins
   ```

2. Add the pipeline script (`Jenkinsfile`) to the root of your GitHub repo:

   ```groovy
   pipeline {
       agent any

       environment {
           DOCKER_IMAGE = "springboot-war-app"
           CONTAINER_NAME = "springboot-war-container"
       }

       stages {
           stage('Clone Repo') {
               steps {
                   git 'https://github.com/YOUR_USERNAME/springboot-war-app.git'
               }
           }

           stage('Build WAR with Maven') {
               steps {
                   sh 'mvn clean package'
               }
           }

           stage('Build Docker Image') {
               steps {
                   sh 'docker build -t $DOCKER_IMAGE .'
               }
           }

           stage('Run WAR in Tomcat Container') {
               steps {
                   sh '''
                       docker rm -f $CONTAINER_NAME || true
                       docker run -d --name $CONTAINER_NAME -p 8081:8080 $DOCKER_IMAGE
                   '''
               }
           }
       }
   }
   ```

3. Create a new Jenkins pipeline job and link it to your GitHub repo.

4. Run the pipeline.

---

### 🌐 Access the App

Visit the deployed servlet web app in your browser:

```
http://<your-jenkins-server-ip>:8081
```

---

### ✅ Final Outcome

* Jenkins pulls your code from GitHub
* Builds the WAR file using Maven
* Builds Docker image with WAR deployed in Tomcat
* Runs the Docker container on port `8081`
* You can access the app via browser at `http://<public-ip>:8081`

---

