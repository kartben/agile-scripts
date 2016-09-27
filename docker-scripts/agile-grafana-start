#!/bin/sh

NAME=agile-grafana
docker rm $NAME
docker run \
        -d \
        -p 3000:3000 \
        --name $NAME \
         fg2it/grafana-armhf:v3.1.1

