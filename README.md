Professional Summary : In this project, I utilized Terraform to automate the deployment of a Docker container running the Nginx web server. Terraform, an Infrastructure as Code (IaC) tool, enables the provisioning and management of infrastructure through configuration files, ensuring consistency and repeatability. By leveraging Terraform's Docker provider, I was able to define the necessary resources in a clear and concise manner, leading to an efficient and scalable deployment.

Overview of Terraform : Terraform is an open-source tool developed by HashiCorp that allows users to define and provision infrastructure using a high-level configuration language. It enables Infrastructure as Code (IaC), meaning infrastructure can be managed using code rather than manual processes. This approach enhances collaboration, reduces errors, and accelerates deployment cycles. Terraform supports multiple providers, allowing integration with various services and platforms, including cloud providers and container management systems.

Step-by-Step Guide to Deploying a Docker Container with Terraform: 

1 > Install Terraform
Ensure Terraform is installed on your machine. You can download it from the Terraform website.
- curl -O https://releases.hashicorp.com/terraform/{VERSION}/terraform_{VERSION}_linux_amd64.zip
- unzip terraform_{VERSION}_*.zip
- sudo mv terraform /usr/local/bin/
- terraform version
          OR
Installing Terraform on Ubuntu via APT:
Update the Package Repository: Open your terminal and update the package list to ensure you have the latest information about available packages:
- sudo apt update

Install Required Packages: Install unzip if it is not already installed, as it will be needed to extract the Terraform binary:
- sudo apt install -y unzip

Add the HashiCorp GPG Key: Import the HashiCorp GPG key to verify the packages:
- wget -qO- https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

Add the HashiCorp APT Repository: Add the HashiCorp repository to your APT sources list:
- echo "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

Update the Package Repository Again: After adding the new repository, update your package list:  
- sudo apt update

Install Terraform: Now, you can install Terraform using the following command:
- sudo apt install terraform

Verify Installation: Check that Terraform is installed correctly by running:
- terraform version


2 > Create a new directory for your Terraform project. This directory will contain your configuration files.

- mkdir terraform-docker  && cd terraform-docker

3 > Create the "main.tf" File, you can name it whatever you want as terraform looks for any file with .tf extention (*.tf)
In your project directory, create a file named main.tf and add the following content:

terraform { 
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "3.0.2"
    }
  }    
}

provider "docker" {
  host = "unix:///var/run/docker.sock"
}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}

resource "docker_container" "mycontainer" {
  image = docker_image.nginx.image_id
  name  = "mycontainer"
}

4 > In the terminal, run the following command to initialize your Terraform project. This command downloads the necessary provider plugins specified in your main.tf file.
- terraform init

5 > Before applying changes, it’s a good practice to run the terraform plan command. This command shows you what changes Terraform will make to your infrastructure.
- terraform plan

6 > Once you are satisfied with the plan output, apply the configuration to create the Docker container. This command will provision the resources defined in your main.tf file.
- terraform apply
- 
When prompted to confirm, type yes and hit Enter or run the command with -y flag.

7 > After Terraform finishes applying the configuration, you can verify that your Docker container is running by executing:

- docker ps
  
You should see your container named mycontainer running in the list.

8 > To remove the resources you created, you can use the terraform destroy command. This command will delete the Docker container and image created by Terraform.
- terraform destroy

When prompted, type yes to confirm the deletion or run the command with -y flag.

Through this project, I demonstrated how Terraform can be effectively used to automate the deployment of Docker containers. By defining infrastructure as code, I achieved a reliable and repeatable deployment process, aligning with best practices in DevOps and cloud computing.









