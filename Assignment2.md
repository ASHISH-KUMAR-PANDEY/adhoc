
## :one: Multiple Containers
### :atom: Commands:

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
### :spiral_notepad: Shell Script:

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


## :two: Container Operations
>> From the above task
>
> Write a combination of *docker and OS commands* to get the list of **name, created** of all containers and store them in a text file
>

### :atom: Commands:

$docker ps -a | awk '{print $2 , $4 $5 $6 , $12 $13 }' > list.txt
$cat list.txt


## :three: Container GUI
>Run a container from **your** custom docker image
>
>> The parent process must be *firefox*
>

### :atom: Commands:
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

## :four: Consumption:
>> From task one
>
> Get **RAM and CPU consumptions** of the *100 containers* and stores in a file

### :atom: Commands:

```
$touch stat.sh
$atom stat.sh
$chmod +x stat.sh
$./stat.sh
```
### :spiral_notepad: Shell Script:
#!/bin/bash

for i in {1..100}
do
  docker stats adhocnw$i >> cpu.txt
done
```
