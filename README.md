# SPARK_Rshiny_Rstudio (https://hub.docker.com/r/angelsevillacamins/spark-rstudio-shiny/)

## Step 1 : Pull image from Docker hub
docker pull angelsevillacamins/spark-rstudio-shiny

## Step 2 : Create a network
docker network create spark_network

## Step 3 :  Create data volume container with a folder to share among the nodes
docker create --net spark_network --name data-share --volume <path on your computer> 

## Step 4.1 : Start Master
docker run -d --net spark_network --name master -p 8080:8080 -p 8787:8787 -p 80:3838  --volumes-from data-share --restart=always   angelsevillacamins/spark-rstudio-shiny /usr/bin/supervisord --configuration=/opt/conf/master.conf

## Step 4.2 : Changing permissions in the share folder of the data volume
docker exec master chmod a+w  <path on your computer>

## Step 5.1: Start worker 01
docker run -d --net spark_network --name worker01 --volumes-from data-share --restart=always  angelsevillacamins/spark-rstudio-shiny /usr/bin/supervisord --configuration=/opt/conf/worker.conf

## Step 5.2 Changing permissions in the share folder of the data volume
docker exec worker01 chmod a+w  <path on your computer>

## Step 6.1: Start worker 02
docker run -d --net spark_network --name worker02 --volumes-from data-share --restart=always  angelsevillacamins/spark-rstudio-shiny /usr/bin/supervisord --configuration=/opt/conf/worker.conf

## Step 6.2 Changing permissions in the share folder of the data volume
docker exec worker02 chmod a+w  <path on your computer>

## Step 7
docker ps
