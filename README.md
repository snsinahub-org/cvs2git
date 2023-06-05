# cvs2git - CVS to Git Repository Converter

This repository is a fork :fork: of [cvs2svn](https://github.com/mhagger/cvs2svn) repository

## Pre-requisites
You need to have followings installed on your computer
* Docker
* Git
* Copy of CVS repository from server and `CVSROOT` folder mush be included.

## How to use
This is a containerized tool to migrate a CVS repository to a Git repo. You can run `cvs2git` inside docker container. You can either pull image from ghcr.io, link provided below, or build image on your machine

### Pull image
Simply run following command in your workstation
`docker pull ghcr.io/snsinahub-org/cvs2git:latest`

Optionally, you can rename image by running 
`docker image tag ghcr.io/snsinahub-org/cvs2git:latest cvs2git:latest`
This helps you to have a shorter image name.


### Build image

* `docker build --network host -t cvs2git .`
* `docker run -it -v /path/to/project:/cvs -v /path/to/wanted/git/repo:/git -v /path/to/log/output:/tmp/cvs_migration cvs2git`


### Starting Docker container
We asume name of pulled or built image is cvs2git:latest
* Syntax: `docker run -it -v <path-to-cvs-repo>:/cvs -v <path-for-migrated-git-repo>:/git cvs2git /bin/bash`
* Example: `docker run -it -v /tmp/migrations/cvs:/cvs -v /tmp/migrations/git:/git cvs2git /bin/bash`

By running above command, you get into docker container interactive shell.

### Migrate CVS to Git
#### 
- Syntax: `cvs2git --blobfile=/git/git-blob.dat --dumpfile=/git/git-dump.dat --username=<some-git-username> --fallback-encoding=ascii <path-to-cvs-repo-in-container> > cvs.log`
- Example: `cvs2git --blobfile=/git/git-blob.dat --dumpfile=/git/git-dump.dat --username=cvs --fallback-encoding=ascii /cvs/cvsws > cvs.log`




### Migration with history
- Migration with History
  - Use the included Dockerfile to access `cvs2git`
  - Convert as mush history as possible
    - [cvs2git](http://clusterfrak.com/devops/git/git_cvs2git/)
  - Push to **GitHub**

You will need access to the entire source code including the `CVSROOT`
Example: /path/to/cvs

```bash
- `/path/to/cvs`
  - inside should be the various projects and `CVSROOT`
```

- Run the command to convert the CVS to Git
  - `cd /cvs/<project>`
  - `cvs2git --blobfile=/git/git-blob.dat --dumpfile=/git/git-dump.dat --username=<SomeUserName> --fallback-encoding=ascii <project> >> /tmp/cvs_migration/<project>.log`
- Once the command finishes, you should have a new git object ready to be created
- `cd /git`
- Create a new empty git repo
  - `git init project`
- Change directory into new Git Repo
  - `cd <project>`
- Import the git data to the repo
  - `cat /git/git-blob.dat /git/git-dump.dat | git fast-import`
- Cleanup the data
  - `git gc --prune=now`
- Add a `.gitignore` to remove CVS history files
  - `echo "CVSROOT" >> .gitignore`
- Add all data to the repo
  - `git add .`
- Update the repo to point to the GitHub repo
  - `git remote add origin http://github.com/<org>/<repo>.git`
  - `git config --global user.email "<kdrogo@yourcompany.com>"`
  - `git config --global user.name "<Khal Drogo>"`
  - `git config --global push.followTags true`
- Create commit message
  - `git commit -m "Conversion from CVS to Git"`
- Push the code to GitHub
  - `git push origin master`
  - `git push --all`
  - `git push --tags`

At this point you should have now pushed all data from the converted CVS repo into **GitHub**!

## Docker Hub
The **Docker** container that is built from this repository is located at `https://hub.docker.com/repository/docker/jwiebalk/cvs2git`

--------------------------------------------------------------------------------

### License
- [License](https://github.com/jwiebalk/docker-cvs2git/blob/master/LICENSE)