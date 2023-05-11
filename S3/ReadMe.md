# Save docker image to file locally
    $ docker image save docker-component > docker-component.tar

# Upload docker image to S3 bucket
    $ aws s3 cp docker-component.tar s3://kumo-article-bucket/

# Greengrass V2 component recipe
    ---
    RecipeFormatVersion: '2020-01-25'
    ComponentName: DockerComponent.S3.
    ComponentVersion: '1.0.0'
    ComponentDescription: 'docker component with sample applicaiton deployment using S3'
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
          Install:
            Script: docker load -i {artifacts:path}/docker-component.tar
          Run:
            Script: docker run --publish 8000:5000 --rm docker-component
        Artifacts:
          - URI: s3://componet-source-bucket/docker-component.tar

# Update GreengrassV2 TokenExchange Role with S3 permissions
![Update GreengrassV2 TokenExchange Role with S3 permissions](/S3/Screenshots/UpdateGreengrassV2TokenExchangeRoleWithS3Permissions.png)

# Created Greengrass component
![Component created](/S3/Screenshots/DockerComponentCreation.png)

# Component deployment
![Component Deployment](/S3/Screenshots/DockerComponentDeployment.png)

# Docker component logs
![Docker component logs](/S3/Screenshots/DockerComponentLogs.png)
