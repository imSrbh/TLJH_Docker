# TLJH_Docker
The Littlest JupyterHub (TLJH) distribution helps you provide Jupyter Notebooks to 1-100 users on a single server.


1. Clone this Repo  
`git clone https://github.com/imSrbh/TLJH_Docker.git`

2. Build a docker image that has a functional systemd in it.  
```
docker build -t tljh-systemd . -f Dockerfile
```
(___do not forget the period after build !___)


3. Run a docker container with the image in the background, while bind mounting your TLJH repository under /srv/src.
```
docker run \
  --privileged \
  --detach \
  --name=tljh-dev \
  --publish 12000:80 \
  --mount type=bind,source=$(pwd),target=/srv/src \
  tljh-systemd
  ```
  
4. Get a shell inside the running docker container.
```
docker exec -it tljh-dev /bin/bash
```

5. Run the bootstrapper from inside the container (see step above): The container image is already set up to default to a dev install, so it’ll install from your local repo rather than from github.
```
python3 /srv/src/bootstrap/bootstrap.py --admin admin:password
```


6. You should be able to access the JupyterHub from your browser now at `http://localhost:12000`. Congratulations, you are set up to develop TLJH!

>If you want to add more user, from admin panal you can create user and when user will be signing in the password of his choice which he uses at that first time sign in, that will will be the password for that user.

Inside the container `/home` -> You can see the user as `jupyter-abc`, `jupyter-xyz`

```
root@277863f1ab7e:/home# ls
jupyter-abc  jupyter-xyz
```


---
## TODO
- script file to automate the user setup process.
