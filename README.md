# docker-git-server

A simple dockerized git repository server. Also available on
[Docker Hub](https://hub.docker.com/r/winlu/docker-git-server/).

## Usage
Start the container:
```
docker run -d -p 1234:22 \
   --name git_server \
   -v git_data:/git/data \
   winlu/docker-git-server
```
Or a simple `docker-compose up` (you might want to edit the source of
the image in).

Add a user:
```
docker exec git_server sh add_git_user.sh winlu "`cat ~/.ssh/id_rsa.pub`"
```

Initialize a repo:
```
ssh localhost -p 1234 "init my_repo.git"
```

Add remote to your git repository:
```
git remote add docker_git_server ssh://localhost:1234/~/my_repo.git
```

And push:
```
git push docker_git_server --set-upstream master
```

## Persistence
It is recommended to use a docker volume for persistence because of
file permission issues. All data (setup users, repositories, ssh server keys)
is kept alive during container removal/upgrade.
