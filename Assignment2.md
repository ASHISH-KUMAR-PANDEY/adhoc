
## TASK 1-Multiple Containers
### Commands:

```
$sudo su
$cd /home/ashish/adhoc/dir001
$docker pull alpine
$docker pull fedora
$docker pull centos
$docker pull java
$docker images
$touch ashish.sh
$atom ashish.sh 
$chmod +x ashish.sh
$./ashish.sh
$docker ps
```
### Shell Script:

#!/bin/bash
#

for i in {1..25}
    do docker run --restart=always --name adhocnw$i -P -d alpine
    sleep 3
done

for i in {26..50}
    do docker run --restart=always --name adhocnw$i -P -d fedora
    sleep 3
done

for i in {51..75}
    do docker run --restart=always --name adhocnw$i -P -d centos
    sleep 3
done

for i in {76..100}
    do docker run --restart=always --name adhocnw$i -P -d java
    sleep 3
done


## TASK 2-Container Operations
### Commands:

$docker ps -a | awk '{print $2 , $4 $5 $6 , $12 $13 }' > list.txt
$cat list.txt


## TASK 3-Container GUI
### Commands:
$touch Dockerfile
$docker build -t fire:v1 .
$docker run -d \
    --name=firefox \
    -p 5800:5800 \
    -v /docker/appdata/firefox:/config:rw \
    --shm-size 2g \
    jlesage/firefox
```
### :spiral_notepad: Dockerfile:
```Dockerfile
FROM jlesage/firefox
MAINTAINER ashishkumarpandey2000@gmail.com , 8209966380
RUN echo "firefox created"

```

## TASK 4-Consumption:
### Commands:

```
$touch stat.sh
$atom stat.sh
$chmod +x stat.sh
$./stat.sh
```
### Shell Script:
#!/bin/bash

for i in {1..100}
do
  docker stats adhocnw$i >> cpu.txt
done
```
