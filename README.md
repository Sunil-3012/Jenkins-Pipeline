# 🚀 Jenkins CI/CD Pipeline for Automated Deployment  

## 📌 Overview  
This repository contains a **Jenkins pipeline** built using **Groovy script**, which automates the entire **CI/CD process**, including code compilation, artifact storage, code quality checks, and deployment on **Apache Tomcat**.  

## 🔹 Pipeline Workflow  
The pipeline follows these steps:  

1. **Clone Code from GitHub** – Jenkins pulls the latest source code.  
2. **Build & Package with Maven** – Compiles the project and generates a `.war` file.  
3. **Upload Artifacts to Amazon S3** – Stores the build output securely.  
4. **Code Quality Analysis with SonarQube** – Ensures security & maintainability.  
5. **Deploy to Apache Tomcat** – Automatically deploys the application.  

## 🔹 Infrastructure Setup  
The pipeline is hosted on **AWS EC2 instances**:  

- 🟢 **Jenkins Server** – Orchestrates the pipeline.  
- 🟢 **SonarQube Server** – Performs static code analysis.  
- 🟢 **Tomcat Server** – Hosts the deployed application.

![Image Alt](https://github.com/Sunil-3012/Jenkins-Pipeline/blob/main/pipeline.png?raw=true)

## 🔹 Tech Stack  
| Technology | Purpose |
|------------|---------|
| **Jenkins** | CI/CD pipeline automation |
| **Groovy** | Pipeline scripting |
| **Maven** | Build & dependency management |
| **Amazon S3** | Artifact storage |
| **SonarQube** | Code quality & security |
| **Apache Tomcat** | Deployment server |
| **AWS EC2** | Cloud infrastructure |

## 🔹 Pipeline Workflow 

### ✅ **1. Clone Source Code from GitHub**  
Clones latest code from the **GitHub repository**.  

```groovy
stage('Clone from GIT') {
    steps {
        git 'https://github.com/Sunil-3012/java-project-maven.git'
    }
}
```
### ✅ **2. Compiled The Project** 
Uses Maven to compile the Java project.

```groovy
stage('compile') {
    steps {
        sh 'mvn compile'
    }
}
```

### ✅ **3. Running Unit Tests** 
Executes unit tests using Maven Surefire Plugin to ensure application stability.

```groovy
stage('test') {
    steps {
        sh 'mvn test'
    }
}
```

### ✅ **4. SonarQube Code Quality Analysis** 
Scans the project for code quality and security vulnerabilities using SonarQube Scanner for Maven.

```groovy
stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('SonarQube') {
            sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
        }
    }
}
```

### ✅ **5. Upload Artifacts to Amazon S3** 
Uploads the packaged `.war` file to an S3 bucket for storage using the Jenkins S3 Publisher Plugin.

```groovy
stage('arthifacts to S3') {
    steps {
        s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, 
        entries: [[bucket: 's3-arthifact-bkt/', sourceFile: '**/*.war', selectedRegion: 'us-east-1', 
        useServerSideEncryption: true]], profileName: 's3creds'
    }
}
```


### ✅ **6. Package the Application** 
Creates a deployable `.war` file using Maven.

```groovy
stage('package') {
    steps {
        sh 'mvn clean package'
    }
}
```


### ✅ **7. Deployed Application to Tomcat Server**
Deploys the `.war` file to Apache Tomcat 9 running on an EC2 instance.

```groovy
stage('deploy') {
    steps {
        deploy adapters: [tomcat9(credentialsId: 'tomcatcreds', path: '', url: 'http://54.226.156.83:8080/')], 
        contextPath: 'MyApp', war: '**/*.war'
    }
}
```
![Image Alt](https://github.com/Sunil-3012/Jenkins-Pipeline/blob/main/Netflix-MyApp.png?raw=true)

## 🌟 Key Benefits  

✅ **Automated CI/CD Workflow** – Streamlines software delivery by automating build, test, and deployment processes.  

✅ **Improved Code Quality & Security** – Integrated **SonarQube** ensures high code quality and detects security vulnerabilities.  

✅ **Secure Artifact Storage** – Utilizes **Amazon S3** to store and manage build artifacts efficiently.  

✅ **Seamless Deployment** – Automatically deploys the application to **Apache Tomcat**, enabling continuous delivery.  

## 🔥 Next Steps  

🚀 **Improve Error Handling & Rollback Strategies** – Implement automated rollback mechanisms to revert deployments in case of failures.  

🐳 **Integrate with Docker & Kubernetes** – Containerize the application and deploy it using **Kubernetes (K8s)** for scalability and orchestration.  

🔄 **CI/CD Pipeline Optimization** – Implement **Jenkins Shared Libraries** and parameterized pipelines for reusable, scalable workflows.  

🔐 **Enhance Security** – Use **AWS IAM roles**, **Secrets Manager**, and **Jenkins Credential Vault** for better access control and security.  

📊 **Performance Monitoring & Logging** – Integrate **Prometheus, Grafana, and ELK Stack** for real-time performance monitoring and centralized logging.  

☁️ **Multi-Cloud Deployment** – Extend the pipeline to deploy across **AWS, Azure, and Google Cloud** for hybrid cloud strategies.  

🛡️ **Automated Testing** – Implement **Selenium, JUnit, and API testing** to enhance automated test coverage and ensure software quality.  

⚡ **Infrastructure as Code (IaC)** – Use **Terraform or AWS CloudFormation** to automate infrastructure provisioning.  

## 🤝 Contributing
Feel free to submit issues or pull requests to improve this pipeline!

## 🚀 Happy DevOps Automating! 🛠️

## Complete Groovy Script

```groovy
pipeline {
    agent any
    stages {
        stage('Clone from GIT') {
            steps {
                git 'https://github.com/Sunil-3012/java-project-maven.git'
            }
        }
        stage('compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
            }
        }
        stage('arthifacts to S3') {
            steps {
                s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 's3-arthifact-bkt/', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: '**/*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: true]], pluginFailureResultConstraint: 'FAILURE', profileName: 's3creds', userMetadata: []
            }
        }
        stage('package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcatcreds', path: '', url: 'http://54.226.156.83:8080/')], contextPath: 'MyApp', war: '**/*.war'
            }
        }
    }
}
```

