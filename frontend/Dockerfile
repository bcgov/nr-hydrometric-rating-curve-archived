### BUILDER IMAGE ###
# syntax=docker/dockerfile:1
FROM python:3.10-slim AS BUILDER

# set environment variables
ENV LANG=C.UTF-8
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# use python venv to copy to the app image later
ENV PATH="/venv/bin:$PATH"

RUN apt-get update && \
    apt-get install -y gcc

# set up venv
RUN python -m venv /venv

# install requirements
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

### APP IMAGE ###
FROM python:3.10-slim AS APP
WORKDIR /app

# set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PIP_DISABLE_PIP_VERSION_CHECK=1
ENV PATH="/venv/bin:$PATH"

# create and switch to the app user
RUN useradd -m rctool
USER rctool

# copy project
COPY --from=BUILDER /venv /venv
COPY . ./app/