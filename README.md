# Containerized Buildkite Template

## File definitions

### .buildkite/pipeline.yml

Contains the steps of the buildkite build. The default step runs "step1.sh" from the "scripts" folder using the docker service entitled "step1" defined in "docker-compose.yml".

### docker-compose.yml

Defines the services that are provided by docker. One service is currently defined using /scripts/Dockerfile-step1 as its Dockerfile.

### scripts/Dockerfile-step1

Using the Ubuntu 18.04 image this dockerfile:
1. Installs python3 and python3-pip 
2. Copies the requirements file defining which packages are to be installed in pip to the container.
3. Copies the pipeline step's script that is to be run into the container.
4. Uses pip to install all the requirements in the requirements file.

### scripts/requirements-step1.txt

Lists all the pip piackages which should be installed in the container. Each line should contain a single package and, ideally, the required versions. For instance:

```
scipy==1.4.1
npm==0.1.1
numpy==1.18.1
```

The current version of a package can be found by searching for it on https://pypi.org/ .

### scripts/step1.sh

This file is the build script that you wish to run for this step of the pipeline

## Adding more steps

To add more steps, firstly add a new command for that step to ./buildkite/pipeline.yml. Then, in docker-compose.yml, define a new service for the new step. Next create a new Dockerfile for the new step in "scripts" along with an associated requirements file and finally the script. You must ensure that:

1. pipline.yml is requiesting the new service in docker-compose.yml and the new pipeline script.
2. docker-compose.yml is pointing to the new Dockerfile
3. The new dockerfile is pointing to the new requirements file (3 times) and the new pipeline script.
