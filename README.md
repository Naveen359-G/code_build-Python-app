--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# code_build-Python-app
Continues Integration - Code Build
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Steps:
1. Go to AWS CodeBuild Console:
Sign in to the AWS Management Console and navigate to the CodeBuild service.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
2. Create a New Build Project:
Click on "Create build project."
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
3. Configure Build Project Settings:
Project Configuration: Enter a unique project name and description.

Source: Choose GitHub as the source provider.

Connect to your GitHub repository and select the branch to build.
Environment: Choose the operating system, runtime, and runtime version.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
=>
Define environment variables using Parameter Store:
DOCKER_REGISTRY_USERNAME: Set to /myapp/docker-credentials/username
DOCKER_REGISTRY_PASSWORD: Set to /myapp/docker-credentials/password
DOCKER_REGISTRY_URL: Set to docker.io
DOCKER_IMAGE_NAME: Set to your specific image name
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Buildspec.yml
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

version: 0.2

env:
  variables:
    DOCKER_USERNAME: /myapp/docker-credentials/username
    DOCKER_PASSWORD: /myapp/docker-credentials/password
    DOCKER_REGISTRY_URL: docker.io
    DOCKER_IMAGE_NAME: 'your image name'

phases:
  install:
    runtime-versions:
      python: 3.11

  pre_build:
    commands:
      - echo "Installing dependencies..."
      - pip install -r requirements.txt

  build:
    commands:
      - echo "Running tests..."
      - echo "Logging in to Docker Hub..."
      - echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin "$DOCKER_REGISTRY_URL"
      - echo "Building Docker image..."
      - docker build -t "$DOCKER_USERNAME/$DOCKER_IMAGE_NAME:latest" .
      - echo "Pushing Docker image to Docker Hub..."
      - docker push "$DOCKER_USERNAME/$DOCKER_IMAGE_NAME:latest"

  post_build:
    commands:
      - echo "Build completed successfully!"

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

4. Review and Create the Build Project:

- Review all the settings, ensuring that environment variables and the buildspec.yml are configured correctly.
- Click on "Create build project."
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

5. Start a Build:

- Trigger a manual build to test the configuration.
- View the build logs to ensure the build runs successfully.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

6. Monitor and Troubleshoot:

- Monitor build history, logs, and status in the CodeBuild console.
- Troubleshoot any build failures by examining build logs and revisiting build configuration.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


