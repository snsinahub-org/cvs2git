# cvs2git - CVS to Git Repository Converter

This repository is a fork of [cvs2svn](https://github.com/mhagger/cvs2svn) repository

## Prerequisites

You need to have followings installed/Copied in your computer
* Docker
* Git
* Copy of CVS repository from server, please make sure `CVSROOT` folder is included.

## How to use

This is a containerized tool to migrate a CVS repository to a Git repo. You can run `cvs2git` inside a docker container. You can either pull image from ghcr.io, link provided below, or clone this repository and build image on your machine

### Pull image

Simply run following command in your workstation

- `docker pull ghcr.io/snsinahub-org/cvs2git:latest`
- Optionally, you can rename the image by running
  - `docker image tag ghcr.io/snsinahub-org/cvs2git:latest cvs2git:latest`


### OR Build image

* `docker build --network host -t cvs2git .`
* `docker run -it -v /path/to/project:/cvs -v /path/to/wanted/git/repo:/git -v /path/to/log/output:/tmp/cvs_migration cvs2git`



### Starting Docker container
We assume name of pulled or built image is cvs2git:latest
* Syntax: `docker run -it -v <path-to-cvs-repo>:/cvs -v <path-for-migrated-git-repo>:/git cvs2git /bin/bash`
* Example: `docker run -it -v /tmp/migrations/cvs:/cvs -v /tmp/migrations/git:/git cvs2git /bin/bash`

By running the above command, you get into the docker container interactive shell.

### Migrate CVS to Git

- Syntax: `cvs2git --blobfile=/git/git-blob.dat --dumpfile=/git/git-dump.dat --username=<some-git-username> --fallback-encoding=ascii <path-to-cvs-repo-in-container> > cvs.log`
- Example: `cvs2git --blobfile=/git/git-blob.dat --dumpfile=/git/git-dump.dat --username=cvs --fallback-encoding=ascii /cvs/cvsws > cvs.log`
- Example with options: `cvs2git --blobfile=/git/git-blob.dat --dumpfile=/git/git-dump.dat --username=cvs --fallback-encoding=ascii /cvs/cvsws --options=cvs2git.options > cvs.log`

### All commands

- Migration with History
  - Use the included Dockerfile to access `cvs2git`
  - Convert as mush history as possible
    - [cvs2git](http://clusterfrak.com/devops/git/git_cvs2git/)
  - Push to **GitHub**
- Run the command to convert the CVS to Git
  - `cd /cvs/<project>`
  - `cvs2git --blobfile=/git/git-blob.dat --dumpfile=/git/git-dump.dat --username=<SomeUserName> --fallback-encoding=ascii <project> >> /tmp/cvs_migration/<project>.log`
- Once the command finishes, you should have a new git object ready to be created
- `cd /git`
- Create a new empty git repo
  - `git init --bare <project>.git`
  - example: `git init --bare cvsws.git`
- Change directory into new Git Repo
  - `cd <project>.git`
- Import the git data to the repo
  - `cat /git/git-blob.dat /git/git-dump.dat | git fast-import`
- Cleanup the data
  - `git gc --prune=now`
 
## Prepare Git repo and push to GitHub
You can either run following commands in docker container or exit from container and run from host computer.

- Update the repo to point to the GitHub repo
  - `git branch -M main`
  - `git config --global user.email "<your-github-account-associated-email>"`
  - `git config --global user.name "<Your GitHub username>"`
  - `git config --global push.followTags true`
- Push the code to GitHub
  - `git push -u origin main`
  - `git push origin --tags`
  
At this point you should have now pushed all data from the converted CVS repo into **GitHub**!
