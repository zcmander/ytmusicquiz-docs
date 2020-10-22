# Production environemnt

Change your current working directory to: `yt-musicquiz\ytmusicquiz-deploy`, which contains production env-related files.

Production environment uses Terraform as a solution to manage infrastructure as code. It has been written to utilize hosted services provided by AWS and does not easily convert to another cloud provider. However, it's possible to do so, because the application uses only open source -software to implement it's features.

Following external software needed to run the application in production environment:
* PostgreSQL -database

AWS-services utilized by the production environment scripts:

 * Amazon ECR - Elastic Container Registry
 * Amazon ECS - Elastic Container Service

The infrastructure uses AWS Fargate -solution to set up an environment that does not need any virtual machine to avoid any unnessecary maintenance costs.


# Steps to set up your environment

1. Set up AWS resource

    terraform apply

To create managed resources to AWS.

2. Copy ECR-URL

Copy ECR-instance URL using AWS Management Console.

Create a new file `.env` with following content:

    DOCKER_REGISTRY=xxxxxxxx.xxx.xxx.xx-xxxx-x.amazonaws.com

3. Build images

Build images using Docker Compose

    docker-compose build

4. Push images to registry

Run:

    docker-compose push
