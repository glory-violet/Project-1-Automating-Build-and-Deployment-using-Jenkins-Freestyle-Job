# Project-1-Automating-Build-and-Deployment-using-Jenkins-Freestyle-Job

This project demonstrates Continuous Integration and Continuous Deployment (CI/CD) automation using Jenkins Freestyle Jobs on AWS EC2 Ubuntu Server. The implementation covers Jenkins installation, GitHub integration, Maven-based application build automation, JUnit testing, deployment automation, artifact archiving, and email notification configuration.

The project includes:

- Jenkins Installation and Configuration
- GitHub Repository Integration
- Jenkins Freestyle Job Creation
- Maven Build Automation
- JUnit Test Execution
- Local Deployment Automation
- Artifact Archiving
- Build Report Visualization
- Email Notification Configuration

---

# Technologies Used

- AWS EC2
- Ubuntu Server
- Jenkins
- Maven
- Git & GitHub
- Java (OpenJDK 17)
- JUnit
- SMTP (Gmail Notification)

---

# GitHub Repository

Repository URL:

```bash
https://github.com/rapetisekhar/jenkins-task
```

---

# Implementation Steps and Screenshots

---

# Step 1 - Setup Ubuntu EC2 Instance

Created Ubuntu EC2 instance with required security group ports for Jenkins and deployment.

### Configuration

- AMI: Ubuntu Server
- Instance Type: t2.micro
- Storage: 20 GB
- Open Ports:
  - 22 (SSH)
  - 8080 (Jenkins)
  - 8081 (Deployment)
  - 25 (SMTP)

### Screenshots

- [EC2 Instance Creation - 1](https://github.com/user-attachments/assets/b44dee3e-df8e-4b51-9878-425c402e1f57)

- [EC2 Instance Creation - 2](https://github.com/user-attachments/assets/87ef2d9c-0740-4bbf-a96b-8f31f1bb3249)

---

# Step 2 - Connect to EC2 Instance using SSH

### Commands Used

```bash
chmod 400 key.pem

ssh -i key.pem ubuntu@<your-public-ip>
```

### Screenshot

- [SSH Connection](https://github.com/user-attachments/assets/d6264f8a-97eb-4f5e-ba22-cd98102e496e)

---

# Step 3 - Configure Hostname

### Command Used

```bash
sudo hostnamectl set-hostname jenkins-server
```

### Screenshot

- [Hostname Configuration](https://github.com/user-attachments/assets/a8380164-1588-4363-827f-e03d22dab812)

---

# Step 4 - Update Linux Packages

### Command Used

```bash
sudo apt update && sudo apt upgrade -y
```

### Screenshot

- [Linux Package Update](https://github.com/user-attachments/assets/26232d41-8f25-4452-87d4-53b693477a64)

---

# Step 5 - Install Java

### Commands Used

```bash
sudo apt install openjdk-17-jdk -y

java --version
```

### Screenshot

- [Java Installation](https://github.com/user-attachments/assets/6ae81bee-b74f-47d3-9e0f-0ebc189531db)

---

# Step 6 - Install Maven

### Commands Used

```bash
sudo apt install maven -y

mvn -v
```

### Screenshot

- [Maven Installation](https://github.com/user-attachments/assets/340d5095-303d-4585-8fe6-bbbd4235937a)

---

# Step 7 - Install Jenkins

### Commands Used

```bash
wget https://pkg.jenkins.io/debian-stable/binary/jenkins_2.504.1_all.deb

sudo apt install fontconfig openjdk-17-jre -y

sudo dpkg -i jenkins_2.504.1_all.deb

sudo apt --fix-broken install -y

sudo dpkg --configure -a
```

---

# Step 8 - Start and Enable Jenkins Service

### Commands Used

```bash
sudo systemctl daemon-reload

sudo systemctl start jenkins

sudo systemctl enable jenkins

sudo systemctl status jenkins
```

### Screenshot

- [Jenkins Service Status](https://github.com/user-attachments/assets/c1878d35-46e7-471e-86f2-66e542a44547)

---

# Step 9 - Launch Jenkins Dashboard

### URL

```bash
http://<public-ip>:8080
```

### Screenshot

- [Jenkins Dashboard](https://github.com/user-attachments/assets/c2008094-3148-4017-8104-b923a2cbf79d)

---

# Step 10 - Install Suggested Plugins and Create Admin User

### Screenshot

- [Suggested Plugins Installation](https://github.com/user-attachments/assets/a5c96697-b57f-42b6-8e7f-9c71a6916234)

- [Admin User Creation](https://github.com/user-attachments/assets/697fdb67-d891-4cc3-ad4c-328c169b57e2)

---

# Step 11 - Install Required Jenkins Plugins

Installed:
- Git Plugin
- Maven Integration Plugin
- Email Extension Plugin

### Screenshot

- [Plugin Installation](https://github.com/user-attachments/assets/930693e3-6897-409c-86c0-0c4c8e39ebea)

---

# Step 12 - Install Git on Jenkins Server

### Commands Used

```bash
sudo apt install git -y

git --version
```

### Screenshot

- [Git Installation](https://github.com/user-attachments/assets/ce3be218-2efb-41a0-8c8b-ef047f8b1ea1)

---

# Step 13 - Generate SSH Keys

### Command Used

```bash
ssh-keygen
```

### Screenshot

- [SSH Key Generation](https://github.com/user-attachments/assets/333b5064-af60-489b-983b-8c4daa565356)

---

# Step 14 - Configure GitHub SSH Authentication

### Command Used

```bash
cat ~/.ssh/id_ed25519.pub
```

### Screenshot

- [GitHub SSH Key Configuration](https://github.com/user-attachments/assets/320654f9-5054-4e04-9680-01f3d483a9a9)

---

# Step 15 - Verify GitHub Connectivity

### Command Used

```bash
ssh -T git@github.com
```

### Screenshot

- [GitHub SSH Verification](https://github.com/user-attachments/assets/2fa1e859-37de-4c56-a8e6-145b92d1d8b4)

---

# Step 16 - Create Jenkins Freestyle Job

### Configuration

- Item Name: jenkins-project-2
- Project Type: Freestyle Project

### Screenshot

- [Freestyle Job Creation](https://github.com/user-attachments/assets/32c6d9c7-0bdd-47e7-b5d3-20151095b422)

---

# Step 17 - Configure GitHub Token Authentication

Generated GitHub Personal Access Token with repository permissions.

### Screenshot

- [GitHub PAT Token](https://github.com/user-attachments/assets/5d5d9c66-9a4e-4bab-849b-0cda818cee76)

---

# Step 18 - Configure Source Code Management (SCM)

Configured Jenkins SCM using GitHub repository URL and credentials.

### Screenshot

- [SCM Configuration](https://github.com/user-attachments/assets/b70100e9-1391-4408-8d25-e24bc38fe83a)

---

# Step 19 - Configure Maven Build

### Maven Goal

```bash
-B clean package -DskipTests
```

### Screenshot

- [Maven Project Configuration](https://github.com/user-attachments/assets/f333023f-1bd4-40dc-8760-52f8175ef5fc)

---

# Step 20 - Configure Deployment Script

### Linux Deployment Script

```bash
#!/bin/bash
echo "--- Deploying Artifact to Local Linux Workspace ---"

cd "$WORKSPACE/target"

for file in *.jar; do
    echo "Starting application: $file"
    nohup java -jar "$file" --server.port=8081 > app.log 2>&1 &
    break
done

echo "Application deployment initialized successfully on port 8081!"
```

### Screenshot

- [Deployment Script Configuration](https://github.com/user-attachments/assets/dd3b6123-ca7b-428b-9287-64ff525920c2)

---

# Step 21 - Configure JUnit Testing

### Test File Path

```bash
src/test/java/com/example/AppTest.java
```

### JUnit Test Code

```java
package com.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class AppTest {
    @Test
    public void shouldAnswerWithTrue() {
        assertTrue(true);
    }
}
```

### Screenshot

- [JUnit Test Step](https://github.com/user-attachments/assets/11945bcc-926d-4de1-b5f7-f38f1237cfdb)

---

# Step 22 - Verify Build Success

### Expected Output

```bash
[INFO] Running com.example.AppTest
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
[INFO] BUILD SUCCESS
```

### Screenshot

- [Build Success Output](https://github.com/user-attachments/assets/bdd20a01-86ef-4155-8287-f031a302017a)

---

# Step 23 - Verify Test Reports

### Linux Path

```bash
cd /var/lib/jenkins/workspace/jenkins-server/target/surefire-reports/

ls -la

cat com.example.AppTest.txt
```

### Screenshot

- [Surefire Reports](https://github.com/user-attachments/assets/89b2d119-0a20-4a59-8e4b-5dd523426862)

---

# Step 24 - Configure Artifact Deployment

### Screenshot

- [Artifact Deployment](https://github.com/user-attachments/assets/8eb40238-338e-4f05-b516-592cd076995d)

---

# Step 25 - Publish JUnit Reports and Graphs

### Configuration

```bash
target/surefire-reports/*.xml
```

### Screenshot

- [JUnit Report Graph](https://github.com/user-attachments/assets/55090eb1-c8a0-4e78-a82c-17a5aa5186f0)

---

# Step 26 - Archive Build Artifacts

### Configuration

```bash
target/*.jar
```

### Screenshot

- [Artifact Archiving](https://github.com/user-attachments/assets/094666eb-f590-4442-afa8-9f5ed17cd4ca)

---

# Step 27 - Configure Email Notifications

### SMTP Configuration

- SMTP Server: smtp.gmail.com
- SMTP Port: 465
- SSL: Enabled

### Screenshot

- [Google App Password Setup](https://github.com/user-attachments/assets/10caa3a1-399f-420e-ba64-1dbcd34929db)

---

# Step 28 - Verify Email Notifications

### Screenshot

- [Email Notification Testing](https://github.com/user-attachments/assets/1c34907d-66b3-44ed-b2e8-7662c8e57119)

---

# Step 29 - Integrate Email Notifications into Jenkins Job

### Screenshot

- [Jenkins Email Notification Integration](https://github.com/user-attachments/assets/1e6e7615-a632-42d3-a185-a0f7155a0a9b)

---

# Outcome

Successfully implemented a complete Jenkins-based CI/CD pipeline using Freestyle Jobs by integrating:

- Jenkins Automation Server
- GitHub Source Code Management
- Maven Build Automation
- JUnit Testing
- Automated Deployment
- Artifact Archiving
- Build Monitoring and Reporting
- Email Notifications

The Jenkins pipeline successfully automated:
- Source code retrieval
- Application build process
- Automated testing
- Artifact generation
- Local deployment
- Build status notifications
