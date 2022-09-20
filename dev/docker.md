## Golang, multi-stage

```dockerfile
#This is the "builder" stage
FROM golang:1.15 as builder
WORKDIR /my-go-app
COPY app-src .
RUN GOOS=linux GOARCH=amd64 go build ./cmd/app-service

#This is the final stage, and we copy artifacts from "builder"
FROM gcr.io/distroless/static-debian10
COPY --from=builder /my-go-app/app-service /bin/app-service
ENTRYPOINT ["/bin/app-service"]
```

## Python

```dockerfile
FROM python:3.10-slim

RUN apt update \
    && apt upgrade -y \
    && apt install -y curl \
        locales \
    && rm -rf /var/lib/apt/lists/*

# RU Locale
RUN sed -i -e 's/# ru_RU.UTF-8 UTF-8/ru_RU.UTF-8 UTF-8/' /etc/locale.gen \
    && locale-gen

RUN pip3 install --no-cache-dir --upgrade pip \
    poetry

RUN useradd --no-create-home --gid root runner

```