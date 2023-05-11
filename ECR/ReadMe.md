# Logged in ECR
    $ aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 1234567890.dkr.ecr.us-east-1.amazonaws.com

# Tagged docker image
    $ docker tag docker-component:latest 1234567890.dkr.ecr.us-east-1.amazonaws.com/docker-component:latest

# Pushed docker image to ECR
    $ docker push 1234567890.dkr.ecr.us-east-1.amazonaws.com/docker-component:latest

# Greengrass V2 component recipe
    ---
    RecipeFormatVersion: '2020-01-25'
    ComponentName: com.example.MyPrivateDockerComponent
    ComponentVersion: '1.0.0'
    ComponentDescription: 'A component that runs a Docker container from a private Amazon ECR image.'
    ComponentPublisher: samba723
    ComponentDependencies:
    aws.greengrass.DockerApplicationManager:
        VersionRequirement: ">=2.0.8 <2.1.0"
        DependencyType: "HARD"
    aws.greengrass.TokenExchangeService:
        VersionRequirement: ">=2.0.3 <2.1.0"
        DependencyType: "HARD"
    Manifests:
    - Platform:
        os: linux
        Lifecycle:
            Run: docker run --publish 8000:5000 --rm 1234567890.dkr.ecr.us-east-1.amazonaws.com/docker-component:latest
        Artifacts:
        - URI: "docker:1234567890.dkr.ecr.us-east-1.amazonaws.com/docker-component:latest"

# Update GreengrassV2 TokenExchange IAM role to add access to the ECR
![Update GreengrassV2 TokenExchange Role with ECR permissions](/ECR/Screenshots/UpdatedGreengrassTokenExchangeRollWithECRPermissions.png)

# Created Greengrass component
![Component created](/ECR/Screenshots/DockerComponentCreation.png)

# Component deployment
![Component Deployment](/ECR/Screenshots/DockerComponentDeployment.png)

# Docker component logs
![Docker component logs](/ECR/Screenshots/DockerComonentLogs.png)