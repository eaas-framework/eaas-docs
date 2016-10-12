# EMiL Docker Demo

This tutorial describes how to setup a EMiL docker instance.  

## Preparations

- Install Docker (https://docs.docker.com/)
  * make sure your default user is allowed to start docker container e.g. for Ubuntu: http://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo 

- Create a folder where all the data should be stored
  * this is for simple setups only, more advanced installations require additional configuration steps
   
  e.g. 
	```
	mkdir eaas-user-data/
	cd eaas-user-data/
	```
  from now on, we  assume that this folder is your current working directory
   
- Provide an image archive
   * The default configuration will look for a folder named `image-archive`. 
   * A simple image-archive can be downloaded here: [http://bwfla.uni-freiburg.de/image-archive.tgz]. To setup your own image archive follow the instructions [here](imageArchive).
   
- Provide an object archive 
   * The default configuration will look for a folder named `objects`.
   * A simple (file-based) object archive can be build using the following structure:
   
     ```
     objects/ 
     |- object1id/ 
     |   \ iso/ 
     |      |- disk1.iso 
     |      |- disk2.iso 
     |- object2id/ 
     |   \ floppy/ 
     |      |- disk1.img 
     ```
   Every folder in `objects` represents a digital object, the folder name the object's id. The object ID can be an arbitrary string.
   Every object may contain subfolders labeled `iso`, `floppy`and `disk`. These folder names encode what kind of images they contain. 

## Local setup 
Download [eaas.sh](eaas.sh).

For a pure local setup (e.g. the service will be available at [http://localhost:8080]) no further setup is required. 

If the service should be bound to an different IP or TCP port, edit eaas.sh and modify the following variables (e.g. 
listening to the FQDN [emulation.solutions] on port 123):

```bash
DOCKER_ENV="-e WEBFQDN=http://emulation.solutions:123"
NETCONF="-p 123:80"
```

## Run EMiL (MacOS / Linux)
Run `eaas.sh /your/path/eaas-user-data`. 

This script will a few directories in the working directory (if not present) and checks the availability of the image and object archive. 

- eaas.sh creates a `log` directory. 
  * This directory will contain the log file of the EaaS application server. 

- eaas.sh creates a `software-archive/` directory. 
  * This directory will be used to store software meta-data. Every software object is a "regular" digital object, available by reference from an object-archive (see [Object Archive](objectArchive)). The software-archive contains only some extra meta-data (attributes) for a digital object. 
  
- eaas.sh creates a `export/` directory.
  * Exported environments and their meta-data are written to this directory.
   
- eaas.sh creates a `emil-environments/` directory.
  * This directory acts as a dummy meta-data repository containing descriptive meta-data for emulation environments provided by an image-archive. 
  The meta-data available from the image-archive describes an emulation environment technically, i.e. to configure an EaaS emulator setup. In contrast 
  EMiL environment meta-data is only used by the frontend, e.g. additional catalog information. 
  Make sure to import environments if you run EMiL the first time. 

- eaas.sh creates a `emil-object-environments` directory.
  * This directory contains similar meta-data as found in `emil-environments/` but describes specific object setups, e.g. a configuration 
  describing customized emulation environments for a specific object. 
  
The script will pull/download the EMiL docker from dockerhub and start the EaaS service. Starting the EaaS service may take some time, typically 30 - 60 sec. Wait until you see a message like:

```
Oct 12 20:00:19 cf682ebf9cd4 root: Successfully deployed "eaas-server.ear".
```

If the default setup has been used, the EMiL user UI is then available under [http://localhost:8080] and the 
EMiL administration UI will be available under [http://localhost:8080]. 
   