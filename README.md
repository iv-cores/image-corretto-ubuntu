# Amazon Corretto JDK + Ubuntu Jammy

This Docker image is designed to containerize the build environments for Java applications using Amazon Corretto JDK on Ubuntu Jammy. While Alpine is great, sometimes you need something a little more feature-rich.

## Features

- **Base OS**: Ubuntu Jammy
- **Java Development Kit**: Amazon Corretto JDK 21

## Usage

The following is a usage example to build a project in the current working directory using the Gradle wrapper.

```sh
docker run \
  --rm \
  --name "build-in-docker" \
  --workdir "/build" \
  --volume "$PWD:/build" \
  --entrypoint "./gradlew" \
  "registry.ivcode.org/corretto-ubuntu:21-jammy" \
  clean build
```
