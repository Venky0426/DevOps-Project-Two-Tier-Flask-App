Two-Tier Flask Application on AWS with Jenkins CI/CD
Author: Venkey HM
Project: DevOps Project — Two-Tier Flask + MySQL Application
Repository: Venky0426/DevOps-Project-Two-Tier-Flask-App
Project Overview
This project demonstrates deployment of a two-tier web application built with Flask and MySQL on an AWS EC2 instance. Docker packages the application and database into containers, Docker Compose manages the services, and Jenkins automates the build and deployment workflow from the GitHub repository.

The project also includes a health endpoint and a MySQL database table used to store messages submitted through the application.
Technology Stack
- Cloud: AWS EC2
- Operating system: Ubuntu
- Application: Python, Flask
- Database: MySQL
- Containers: Docker, Docker Compose
- CI/CD: Jenkins Pipeline
- Version control: Git and GitHub
- Verification: curl, Docker health checks, MySQL CLI
  
Architecture
Developer
   |
   | Push code
   v
GitHub repository
   |
   | Jenkins checks out source code
   v
Jenkins Pipeline on EC2
   |
   | Build Docker image
   | Deploy with Docker Compose
   | Check application health
   v
Docker Compose
   |
   +-----------------------+
   |                       |
   v                       v
Flask application       MySQL database
(container)             (container)
   |                       |
   +---- SQL connection ---+
                           |
                    mysql_data volume
Jenkins and the application are hosted on the EC2 environment used for this project. The Flask service communicates with MySQL through the Docker Compose network.
Project Features
- Flask web application connected to a MySQL database.
- Containerized services managed by Docker Compose.
- Persistent MySQL data using a named Docker volume.
- Health endpoint at /health.
- Jenkins pipeline stages for cloning the repository, building the image, deploying services, and verifying application health.
- GitHub-based source control and deployment workflow.
Repository Structure
DevOps-Project-Two-Tier-Flask-App/
├── app.py
├── Dockerfile
├── docker-compose.yml
├── Jenkinsfile
├── requirements.txt
├── templates/
│   └── index.html
└── README.md
Your repository may contain additional files, such as .gitignore or configuration files.
Jenkins Pipeline
The Jenkins pipeline used in this project has these stages:
1. Clone repository — checks out the main branch from GitHub.
2. Build image — runs Docker Compose to build the Flask application image.
3. Deploy — starts or updates the application services using Docker Compose.
4. Verify deployment — sends a request to http://localhost:5000/health and retries if the application is not ready immediately.
A successful health response observed during testing was:
HTTP/1.1 200 OK
Content-Type: application/json

{
  "status": "ok"
}
The pipeline should be run from Jenkins after changes are pushed. Automatic triggering on every push requires an appropriate GitHub webhook or Jenkins polling configuration.
Database Verification
The MySQL database used by the application is named devops. The messages table was inspected successfully.
Table schema
USE devops;
SHOW TABLES;
DESCRIBE messages;
The table has the following columns:
Column	Type	Constraints
id	int	Primary key, NOT NULL, auto-increment
message	text	Nullable


Query the stored messages
SELECT * FROM messages;
At the time of verification, the query returned five test records:
ID	Message
1	Hello from Docker
2	Second database test
3	hi
4	hi
5	Send ur mail


These are the records present during that test; the table contents may change as users submit new messages.
Data persistence
The Compose configuration uses a named MySQL volume to persist database files across container recreation. Avoid running docker compose down -v unless you intentionally want to remove the named volumes and understand the data-loss consequences.
Useful Commands
Run these commands from the project directory on the EC2 instance.
# Check running containers
docker ps

# Check Compose service status
docker compose -p devops-project-two-tier-flask-app ps

# Check the Flask health endpoint from the EC2 instance
curl -i http://localhost:5000/health

# View recent application logs
docker logs --tail 100 two-tier-app

# View recent MySQL logs
docker logs --tail 100 mysql
To inspect the database, connect to the MySQL container using the credentials configured for your environment, then run the SQL statements in the Database Verification section. Do not put real passwords or secret values in this README.
Run the Application
1. Clone this repository onto an Ubuntu host with Docker and the Docker Compose plugin installed.
2. Configure the required database environment variables securely for your environment.
3. From the project directory, validate the Compose configuration:
   docker compose config --quiet
4. Build and start the services:
   docker compose up -d --build
5. Check the services and health endpoint:
   docker compose ps
   curl -i http://localhost:5000/health
The Compose project name or environment configuration may differ between deployments. Ensure the required variables are available before starting the services.
Security Notes
- Do not commit .env files, passwords, access keys, private keys, or other secrets to GitHub.
- Keep .env excluded through .gitignore and verify that it is not tracked with git ls-files .env.
- Restrict inbound EC2 security-group rules to the minimum required ports and trusted sources. Avoid exposing Jenkins or MySQL publicly unless there is a specific, secured requirement.
- Use a persistent named volume for MySQL data and take backups before making database or infrastructure changes.
Conclusion
This project provided hands-on practice with AWS EC2, Docker, Docker Compose, MySQL, GitHub, and Jenkins-based CI/CD. The deployment was verified with a successful Flask health check, and the MySQL devops.messages table was inspected with five test records at the time of testing.
