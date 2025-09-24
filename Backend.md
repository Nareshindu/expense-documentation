# Expense Tracker - Backend

This is the Spring Boot backend for the Expense Tracker Portal. It provides RESTful APIs for managing Income and Expenses records.

---

## Technologies Used

- **Java 1.8+**
- **Spring Boot**
- **Maven**
- **PostgreSQL**
- **Spring Data JPA**
- **Spring Security**

---

## Step-by-Step Installation and Setup

### 1. Install Java 8 (OpenJDK 1.8)

Java is required to run the backend. On RHEL-based systems, install Java 8 using:
```
sudo dnf install -y java-17-openjdk java-17-openjdk-devel
```
**Verify the installation:**
```
java -version
```
You should see output showing Java 1.8 is installed.

**Set JAVA_HOME Environment Variable:**
Add the following lines to your `~/.bashrc` or `/etc/profile.d/java.sh` file:
```
sudo vi ~/.bashrc
```
```
sudo vi /etc/profile.d/java.sh
```
```
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
export PATH=$JAVA_HOME/bin:$PATH
```
Reload the file to apply changes:
```
source ~/.bashrc
```
# or
```
source /etc/profile.d/java.sh
```

---

### 2. Install Apache Maven

Maven is used to build and manage the project dependencies.

**Manual Installation:**
```
cd /opt
sudo wget https://archive.apache.org/dist/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
sudo tar -xvzf apache-maven-3.9.6-bin.tar.gz
sudo mv apache-maven-3.9.6 apache-maven
```
**Set Maven Environment Variables:**
Create or edit `/etc/profile.d/maven.sh` and add:
```
sudo vi /etc/profile.d/maven.sh
```
```
export M2_HOME=/opt/apache-maven
export PATH=${M2_HOME}/bin:${PATH}
```
Reload:
```
source /etc/profile.d/maven.sh
```
**Verify Maven Installation:**
```
mvn -version
```
You should see Maven version output.

**Alternative (YUM):**
```
sudo yum install maven -y
```

---
### 3. Download and Configure the Application

**Clone the repository:**
```
sudo git clone https://github.com/Nareshindu/expense-backend.git
cd /opt/expense-backend
sudo chown -R ec2-user:ec2-user /opt/expense-backend
```

**Edit database configuration:**
Open the application properties file:
```
sudo vi /opt/expense-backend/src/main/resources/application.properties
```
Update the following lines with your database details:
```
spring.datasource.url=jdbc:postgresql://<DB_IP>:5432/expense_db
spring.datasource.username=expense
spring.datasource.password=Admin@123
```
Replace `<DB_IP>`, `<username>`, and `<password>` with your actual PostgreSQL server details.

---
### 4. Build and Run the Backend

Navigate to the project directory. To ensure a clean build, delete the old `target` directory (if it exists) and then build the package:
```
cd /opt/expense-backend/
mvn clean install
```
After a successful build, you can run the backend using:
```
mvn spring-boot:run
```
Or, if you want to run the packaged JAR directly (after building):
```
java -jar target/java -jar target/expensetracker-1.0.0.jar
```
The backend will start on the default port (usually 8080).

---

### 5. Set Up as a Systemd Service (Recommended for Production)

To manage the backend as a service, create a systemd service file:

```
sudo vi /etc/systemd/system/backend.service
```
Paste the following content:
```
[Unit]
Description=Java Backend Service
After=network.target

[Service]
User=ec2-user
Group=ec2-user
WorkingDirectory=/opt/expense-backend
ExecStart=/usr/bin/java -jar /opt/expense-backend/target/expensetracker-1.0.0.jar
Restart=always
RestartSec=5
SyslogIdentifier=java-backend

[Install]
WantedBy=multi-user.target
```

**Load and start the service:**
```
sudo systemctl daemon-reload
sudo systemctl enable backend
sudo systemctl start backend
```

**To check the status of the service:**
```
sudo systemctl status backend
```

---

### 6. Access the expense Web-Application

If you are using an EC2 instance, open your instance's public IP address in the browser to access the application. For example, if your public IP is `18.234.229.147`, open:

```
http://18.234.229.147:8080/
```

---

### 7. Verification from Backend Server (PostgreSQL Client)

You can check data using the PostgreSQL client (`psql`).

**Add PostgreSQL YUM Repository:**
This enables access to PostgreSQL versions from the official PostgreSQL repo.
```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

**Disable the default PostgreSQL module:**
Prevents RHEL from installing the default older PostgreSQL version.
```
sudo dnf -qy module disable postgresql
```

**Install postgresql13 Client Only:**
Installs the psql command-line client without installing the database server.
```
sudo dnf install -y postgresql13
```

**Verify the client installation:**
```
psql --version
```
You should see something like:
```
psql (PostgreSQL) 13.x
```

**To test the remote connection from backend server:**
```
psql -h <dbserver-ip-addr> -U <dbusername> -d <dbname>
```
Example:
```
psql -h 172.31.45.69 -U expense -d expense_db
```

---