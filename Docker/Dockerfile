# syntax=docker/dockerfile:1

FROM python:3.8-slim-buster

WORKDIR /main

COPY requirements.txt requirements.txt

RUN apt-get update
RUN apt-get install default-jdk -y

RUN pip3 install -r requirements.txt

COPY . .

CMD [ "python3", "-m" , "main", "run", "--host=0.0.0.0"]