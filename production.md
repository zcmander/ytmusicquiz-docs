# Production environment

Change your current working directory to: `yt-musicquiz\ytmusicquiz-deploy`, which contains production env-related files.

The production environment uses Terraform as a solution to manage infrastructure as code. It has been written to utilize hosted services provided by AWS and does not easily convert to another cloud provider. However, it's possible to do so, because the application uses only open-source -software to implement its features.

Following external software needed to run the application in the production environment:
* PostgreSQL -database
* Redis -database

AWS-services utilized by the production environment scripts:

 * Amazon ECR - Elastic Container Registry
 * Amazon ECS - Elastic Container Service

The infrastructure uses AWS Fargate -solution to set up an environment that does not need any virtual machine to avoid any unnecessary maintenance costs.


# Steps to set up your environment

### 1. Set up AWS resource:

Run the following command:

    > terraform apply

    [... snip ...]

    Plan: 24 to add, 0 to change, 0 to destroy.

    Do you want to perform these actions?
      Terraform will perform the actions described above.
      Only 'yes' will be accepted to approve.

    Enter a value: yes

    [... snip ...]

    Apply complete! Resources: 24 added, 0 changed, 0 destroyed.

    Outputs:

    docker_registry = xxxxxx.dkr.ecr.xxx.amazonaws.com
    http_url = ytmusicquiz-lb-xxxxxx.xxx.elb.amazonaws.com


Execution will take a long time if this is the first time you're building  an environment to AWS.

### 2. Prepare Docker environment

Copy `docker_registry`-URL from the Output values and create a new file
called `.env` with the following content:

    DOCKER_REGISTRY=xxxxxxxx.xxx.xxx.xx-xxxx-x.amazonaws.com

### 3. Build images

Build images using Docker Compose

    docker-compose build

This will build the application from scratch and save the images on your
local development machine.

### 4. Push images to the ECR registry

Make sure you have valid authentication:

    > aws2.exe ecr get-login
    docker login ...
    > docker login ...
    Login Succeeded

(Execute the output of `aws2`)

Finally, use Docker compose to push the images:

    docker-compose push

### 5. Navigate to the application

Use `http_url` from the console output of `terraform apply` command and open it with the browser.

You should see the front page of the application.

Now you can access the Admin Panel using `/admin/`-URL , which brings the login page. The default admin credentials in:

    admin:adminpass

Make sure you change the password after login!