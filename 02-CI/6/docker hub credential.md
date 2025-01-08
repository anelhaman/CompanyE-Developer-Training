# Docker Hub Credentials in Jenkins

## Overview
To securely interact with Docker Hub from Jenkins, we need to configure Jenkins to use credentials for authenticating to Docker Hub. The credentials will be stored within Jenkins and referenced in the Jenkinsfile to securely push and pull Docker images.

## Steps to Set Up Docker Hub Credentials in Jenkins

1. **Create a Personal Access Token in Docker Hub**:
    - Go to your [Docker Hub](https://hub.docker.com/) account.
    - Navigate to **Account Settings** > **Security** > **New Access Token**.
    - Create a new personal access token. This token will be used as the password in Jenkins credentials.

2. **Add Docker Hub Credentials to Jenkins**:
    - Go to **Manage Jenkins** > **Manage Credentials**.
    - Under **(global)**, click on **(global)** > **Add Credentials**.
    - In the **Kind** field, choose **Username with password**.
    - **Username**: Enter your Docker Hub username.
    - **Password**: Enter the Docker Hub personal access token you generated.
    - **ID**: Set the ID to `docker-hub-credentials` (or any name you prefer). This ID will be referenced in the Jenkinsfile.
    - Click **OK** to save the credentials.

    > **Important**: Ensure that the **ID** is set to `docker-hub-credentials` (or the name you choose) because it will be used to reference the credentials in the Jenkinsfile.

## Explanation of `docker-hub-credentials` in Jenkinsfile

Once the credentials are added to Jenkins, you can reference them in your Jenkinsfile using the following syntax:

```groovy
docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
    docker.image(DOCKER_IMAGE_NAME).push(DOCKER_TAG)
}
