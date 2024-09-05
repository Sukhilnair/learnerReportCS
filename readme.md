# Learner Report CS
## Project Overview
This project is a MERN stack application consisting of a backend, frontend, Helm charts for Kubernetes deployment, and Jenkins configuration for CI/CD. The project structure is as follows:

- learnerReportCS_backend: Contains the backend code (Node.js/Express.js).
- learnerReportCS_frontend: Contains the frontend code (React.js).
- learnerReportDeployment: Contains Kubernetes deployment files.
- learnerReportHelm: Contains Helm charts for deployment.
- learnReport.jenkins: Jenkins pipeline configuration for CI/CD.

## Directory Structure

```bash
learnerReportCS/
├── learnerReportCS_backend        # Backend code repository
├── learnerReportCS_frontend       # Frontend code repository
├── learnerReportDeployment        # Kubernetes deployment files
├── learnerReportHelm              # Helm charts for Kubernetes
└── learnReport.jenkins            # Jenkins pipeline configuration
```

## Setup Instructions
### 1. Clone the Repositories
Clone the main repository which includes submodules for frontend and backend:

```bash
git clone --recurse-submodules https://github.com/Sukhilnair/learnerReportCS.git
```
### 2. Backend Setup
Navigate to the learnerReportCS_backend directory. Fill the enviorment variables in `config.env` and follow these steps:

```bash
cd learnerReportCS_backend
vim config.env # update the env values here
npm install
node index.js
```
### 3. Frontend Setup
Navigate to the learnerReportCS_frontend directory and follow these steps:

```bash
cd learnerReportCS_frontend
npm install
npm start
```
### 4. Docker Build and Push
Build and Push Backend Docker Image
#### 1.Navigate to Backend Directory:

```bash
cd learnerReportCS_backend
```
#### 2.Build the Docker Image:

```bash
docker build -t ${DOCKER_HUB_REPO}:backend-${VERSION} .
```
Replace  `${DOCKER_HUB_REPO}` with the docker hub repository tag (e.g., sukhilnair/learnerreport).

Replace `${VERSION}` with the desired version tag (e.g., v1.0.0).

#### 3.Log in to Docker Hub:

```bash
docker login
```
Enter your Docker Hub username and password when prompted.

#### 4.Push the Docker Image:

```bash
docker push ${DOCKER_HUB_REPO}:backend-${VERSION}
```
Build and Push Frontend Docker Image
#### 5.Navigate to Frontend Directory:

```bash
cd ../learnerReportCS_frontend
```
#### 6.Build the Docker Image:

```bash
docker build -t ${DOCKER_HUB_REPO}:frontend-${VERSION} .
```
Replace  `${DOCKER_HUB_REPO}` with the docker hub repository tag (e.g., sukhilnair/learnerreport).

Replace `${VERSION}` with the desired version tag (e.g., v1.0.0).

#### 7.Log in to Docker Hub (if not already logged in):
```bash
docker login
```
Enter your Docker Hub username and password when prompted.

#### 8.Push the Docker Image:
```bash
docker push ${DOCKER_HUB_REPO}:frontend-${VERSION}
```


### 5. Kubernetes Deployment
The learnerReportDeployment directory contains Kubernetes manifests for deploying the application.

To deploy the application,use the following command:

```bash
cd learnerReportCS_backend
```
Update the docker image in deployment files.
```bash
kubectl apply -f learnerReportDeployment/
```
### 6. Helm Charts
The learnerReportHelm directory contains Helm charts to streamline the deployment.

To install or upgrade the Helm release, use:

```bash
helm upgrade --install learnerreport ./learnerReportHelm --set backend.image=${DOCKER_HUB_REPO}:backend-${VERSION} --set frontend.image=${DOCKER_HUB_REPO}:frontend-${VERSION}
```
### 7. Jenkins Pipeline
The `learnReport.jenkins` file contains the Jenkins pipeline configuration for building and deploying the application.

Ensure Jenkins is configured to use this pipeline script for automated CI/CD. The script builds Docker images, pushes them to Docker Hub, and deploys the application to Kubernetes using Helm.

### 8. Configure Docker Hub Credentials in Jenkins
To ensure Docker Hub credentials are correctly configured in Jenkins, follow these steps:

#### 1. Obtain Docker Hub Credentials:

- Log in to [Docker Hub](https://hub.docker.com/) and obtain your Docker Hub username and password. For enhanced security, consider creating and using an access token instead of your password.
#### 2. Log in to Jenkins:

- Open your Jenkins instance in a web browser and log in.
#### 3. Navigate to Credentials Configuration:

- From the Jenkins dashboard, click on `Manage Jenkins`.
- Select `Manage Credentials` (or `Credentials` if you’re using Jenkins with a newer interface).
#### 4. Select the Appropriate Domain:

- You can add credentials to the global domain or a specific domain. To add them globally, click on `(global)`.
#### 5. Add New Credentials:

- Click on `Add Credentials`.
#### 6. Configure Docker Hub Credentials:

- Kind: Select `Username with password`.
- Scope: Choose `Global` (or `System` depending on your requirements).
- Username: Enter your Docker Hub username.
- Password: Enter your Docker Hub password or access token.
- ID: Provide a unique ID for these credentials (e.g., `dockerhub-credentials`).
- Description: Optionally, add a description for easier identification.
#### 7. Save Credentials:

- Click `OK` to save your credentials.
## Additional Information
Kubernetes Context: minikube
Jenkins Credentials ID: Ensure the ID matches the one used in the Jenkins pipeline script.
## Troubleshooting
- **Base64 Encoding Errors**: Ensure all secret values in the Helm chart are base64-encoded.
- **Deployment Issues**: Verify Kubernetes configurations and Helm chart paths.