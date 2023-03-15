CREATE A FILE CALLED Dockerfile with the following content:
```
FROM    library/alpine:latest
RUN     apk add php
RUN     apk add git
RUN     git clone https://github.com/academiaonline/phpinfo
RUN     apk add curl
RUN     apk add links

ENTRYPOINT ["php"]
CMD ["-f","phpinfo/src/index.php","-S","0.0.0.0:8080"]
```
CREATE THE CONTAINER IMAGE FROM THE Dockerfile:
```
sudo docker build --file Dockerfile --tag library/alpine:test-dockerfile /mnt/
```
CREATE THE CONTAINER FROM THAT IMAGE:
```
ENTRYPOINT=php
CMD=' -f phpinfo/src/index.php -S 0.0.0.0:8080 '
sudo docker run --detach --entrypoint ${ENTRYPOINT} --name test --publish 80:8080 --tty library/alpine:test-dockerfile ${CMD}
```
FROM THE VM:
```
curl localhost:80/phpinfo/src/index.php
```
FROM THE VM:
```
sudo docker exec test curl localhost:8080/phpinfo/src/index.php
```
FROM THE VM OPEN AN INTERACTIVE TERMINAL INSIDE THE CONTAINER:
```
sudo docker exec --interactive --tty test sh
```
FROM INSIDE THE CONTAINER:
```
curl localhost:8080/phpinfo/src/index.php
links http://localhost:8080/phpinfo/src/index.php
```
