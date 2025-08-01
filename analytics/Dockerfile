FROM public.ecr.aws/docker/library/python:3.12-slim-bookworm

USER root

WORKDIR /src

COPY ./requirements.txt requirements.txt

RUN apt update -y && apt install -y build-essential libpq-dev

RUN pip install -r requirements.txt

COPY . .

CMD python app.py
