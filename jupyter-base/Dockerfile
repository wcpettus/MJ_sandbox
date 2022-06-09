FROM python:3.9.13-slim-bullseye

# Dependencies of psutil for notebook
RUN apt-get update && apt-get install -y gcc python3-dev

RUN pip3 install --upgrade pip && pip3 install notebook
