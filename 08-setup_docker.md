## Docker Installation on CentOS server
##### Referance URL : https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository
### Pre-requisites

Please follow below steps to install docker CE on CentoOS server instance. For RedHat only Docker EE available

1. Install the required packages.

   ```sh
   sudo yum install -y yum-utils \
   device-mapper-persistent-data \
   lvm2
   ```

1. Use the following command to set up the stable repository.

   ```sh
   sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
   ```

### INSTALLING DOCKER CE

1. Install the latest version of Docker CE.
   ```sh
   sudo yum install docker-ce docker-ce-cli containerd.io
   ```

   Note: If prompted to accept the GPG key, verify that the fingerprint matches
060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.

1. Start Docker.
   ```sh
   sudo systemctl start docker
   ```

1. Verify that docker is installed correctly by running the hello-world image.
   ```sh
   sudo docker run hello-world
   ```

## Configure the plugins and start building Docker Images.
Our 1st step is to configure Docker plugin. Whenever a Jenkins build requires Docker, it will create a “Cloud Agent” via the plugin. The agent will be a Docker Container configured to talk to our Docker Daemon.The Jenkins build job will use this container to execute the build and create the image before being stopped. The Docker Image will be stored on the configured Docker Daemon. The Image can then be pushed to a Docker Registry ready for deployment.
1. Once you are inside the Jenkins Dashboard, select Manage Jenkins on the left.
1. On the Configuration page, select Manage Plugins.
1. Manage Plugins page will give you a tabbed interface. Click Available to view all the Jenkins plugins that can be installed.
1. Using the search box, search for Docker plugin. There are multiple Docker plugins, select Docker plugin using the checkbox.
1. Click Install without Restart at the bottom.

The configuration would be used by the plugin which Docker Image to use the agent and which Docker daemon to run the containers and builds on.The plugin treats Docker as a cloud provider, spinning up containers as and when the build requires them.
### Configure Docker agent
1. On the Jenkins Dashboard, select Manage Jenkins.
1. Select Configure System to access the main Jenkins settings.
1. Click on the link `the cloud configuration has moved to a separate configuration page.`
1. Select Docker from the dropdown called Add a new cloud.
1. Click on `Docker cloud details`
1. Add the URI as default value `unix:///var/run/docker.sock`
1. Test connection and save

In some cases you need to add the `jenkins` user to the `docker` group to get the permission to run docker from jenkins. You may need to logout and relogin to jenkins
to do so, on your shell execute:
```
sudo usermod -a -G docker jenkins
```

## Update the Project pipeline to build and publish the docker image

In the project pipeline configuration:

1. Add a post build step
1. choose `Build/Push docker image`
1. Choose the cloud already set in the previous step
1. choose the dockerhub image and set the account credentials
1. Build the project.
You should now see the image built and published on dockerhub
