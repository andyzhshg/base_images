# Ubuntu with ssh and sudo support

## How to build

```shell
docker build -t image-name:tag .
```

## How to run

```shell
docker run \
    --name container_name \
    -p ${SSH_PORT}:22 \ 
    -e SSH_USERNAME=${USER_NAME} \
    -e AUTHORIZED_KEYS="$(cat ~/.ssh/id_rsa.pub)" \
    -d \
    image-name:tag
```
