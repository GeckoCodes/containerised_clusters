# Introduction 
This repo is built off the back of a POC looking at containerising clusters for databricks. The repo contains some example Dockerfiles/ folders that can be used to build container base images which can be customised.

# Benefits of Containerisation
There are some key benefits to this 
which are outlined below:

- Library customization: you have full control over the system libraries you want installed 
    - These are inherently installed as part of the image therefore no library management is required in the UI.
    - Clusters pulling from the same registry location will have consistent versioning.

- Golden container environment: your Docker image is a locked down environment that will never change.

- Docker CI/CD integration: you can integrate Databricks with your Docker CI/CD pipelines.

# Contents
There are some different examples in this repo for different requirements including:
- A Python only cluster example.
- An R only cluster example.
- A Python and R cluster example.
- Example of actual Popeye cluster.
- Example of cluster with custom libraries (Gecko and Armadillo)

# Getting started
To start using this repo, follow the below instructions:

1. Firstly you must have Docker installed in order to build images and push to remote registries - it can be downloaded from: https://www.docker.com/get-started
2. Once Docker is installed and running, start Powershell as Admin and cd to the folder you wish to build as an image.
3. The library requirements can be edited in the `requirements.txt` / `r_packages.sh`
4. Run a `docker build .` to build the image - this may take some time dependent on the volume of requirements.
5. The image can then be pushed to a remote registry in order to be used for cluster creation
    - DockerHub:
        - Firstly create the repo in DockerHub (will need an account etc. https://hub.docker.com/)
        - Will need to tag the image locally with `docker image tag image_id account_name/repo_name:tagname`
        - Can then push with `docker push account_name/repo_name:tagname`

    - Azure Container Registry:
        - For ACR you will need to log in to a registry using azure cli
        - It is simple to do and the following guide can be followed in order to do this: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli?tabs=azure-cli

6. Once the image has been pushed then databricks can access the registry and create a cluster from the image.
 

# Further work
Some final thoughts on how this work could be extended:
- The cluster definitions could be stored in a centralized container registry and updates to library requirements done via pushes to the ACR.
- The databricks cluster definition (eventually in centralised TF definition) could then just define the 'latest' image version and always pull this on start up.
- Could integrate CI/CD pipelines 

