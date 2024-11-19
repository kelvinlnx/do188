# Trying out ncat server and client

go to the ncat directory
```
cd ncat
```
create the abc network & ensure the dns service is enabled
podman network create abc
podman network inspect abc

# create the server and client images
podman build -t nc-server:1.0 -f Containerfile-server
podman build -t nc-client:1.0 -f Containerfile-client

# create the server container
podman run -d --rm --net abc --name server nc-server:1.0
# create the client container passing the dns name of the server as environment variable to the client
podman run -d --rm --net abc --name client -e NC_SVR=server nc-client:1.0

# check the output produced by the server
podman logs server

# Cleanup
podman stop server client
podman network delete abc
podman rmi nc-server:1.0 nc-client:1.0
