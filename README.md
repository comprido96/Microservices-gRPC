# Auth Microservice

## Project Description
This is a microservice app consisting of 2 services, an authentication service and a health check service. Plus, we have a client that can communicate with the auth service.

The auth service has three primary features:
1. Sign in
2. Sign up
3. Sign out

## Learning Objectives
In this project, we aim to learn and practice the following:
* Designing, building, and deploying microservices
* Using gRPC to communicate between microservices
* Monitoring the health of microservices
* Setting up continuous integration & continuous deployment
* Using session based authentication
* Writing testable code
* Organizing code using modules
* Navigating and contributing to an existing code base

## Terminologies

__Session based authentication__

[Session based auth](https://www.geeksforgeeks.org/session-vs-token-based-authentication/) works by giving the client a session token which can be used in subsequent requests to authenticate the user.

__Microservices__

[Microservices](https://microservices.io/) is an architectural style that structures an application as a collection of services that are independently deployable, loosely coupled, organized around business capabilities, and owned by a small team.

__CI/CD__

[CI/CD](https://www.redhat.com/en/topics/devops/what-is-ci-cd) (Continuous Integration/Continuous Delivery or Continuous Deployment) is a set of practices and techniques that help software development teams deliver high-quality software faster and more reliably. Continuous Integration refers to the process of frequently merging code changes from multiple developers into a central repository and running automated tests to detect any integration issues early on. Continuous Delivery/Deployment takes this a step further, automating the entire software release process, from building and testing to deploying the application to production. These practices help teams deliver software more frequently and with higher quality, reducing time-to-market and increasing customer satisfaction.


__Code Overview__

The services communicate via [gRPC](https://grpc.io/) & [Protocal Buffers (A.K.A Protobufs)](https://protobuf.dev/). gRPC is a Remote Procedure Call (RPC) framework that allows us to call functions implemented on a remote service the same way we would call functions implemented locally. Protobufs are a way to serialize structured data. Similar to XML or JSON except a lot smaller, faster, and more powerful. Protobufs define the gRPC services in order to automatically generate Rust code that implement the services.

## Third Party Libraries

Rust has a minimal runtime, which means several third-party libraries are needed to implement our project.

### tokio

[tokio](https://tokio.rs/) is an asynchronous runtime for the Rust programming language.

### tonic, prost, and tonic-build

[tonic](https://crates.io/crates/tonic) is a Rust implementation of gRPC. It is composed of three main components: the generic gRPC implementation, the high performance HTTP/2 implementation and the codegen powered by [prost](https://crates.io/crates/prost).

[tonic-build](https://crates.io/crates/tonic-build) is a development dependency used inside the build script (see `build.rs` below) to compile proto files via `prost` and generate service stubs and proto definitions for use with tonic.

### pbkdf2 & rand_core

[pbkdf2](https://crates.io/crates/pbkdf2) and [rand_core](https://crates.io/crates/rand_core) are used to hash passwords.

### uuid

[uuid](https://crates.io/crates/uuid) is used to generate unique identifies for each user. It is also used within tests to generate unique strings.

### clap

[clap](https://crates.io/crates/clap) is a command-line parser. It is used to create the stand alone client.

## Project Structure

```text
/[root folder]
    |__ proto
        |__ authentication.proto
    |__ src
        |__ /auth-service
            |__ main.rs
        |__ /client
            |__ main.rs
        |__ /health-check-service
            |__ main.rs
    |__ build.rs
    |__ Cargo.toml
```

**proto/authentication.proto**

This is the authentication service using Protocal Buffers is defined.

**build.rs**

This file is a [build script](https://doc.rust-lang.org/cargo/reference/build-scripts.html) used to compile our Protocal Buffers into Rust code. Cargo runs this build script before compiling the source code.

**auth-service/main.rs**

This is the entry point of the authentication service.

**health-check-service/main.rs**

This is the entry point of the health check service.

**client/main.rs**

This is the entry point of the client.


## Usage

binaries can be run with:
```bash
cargo run --bin auth
```
```bash
cargo run --bin health-check
```
```bash
cargo run --bin client
```

__NOTE:__ You can also use [cargo watch](https://github.com/watchexec/cargo-watch) to automatically restart services when source files change.

First build the project by running `cargo build`.

Then execute the following commands in different terminal windows:

```bash
cargo watch -c -q -w src/auth-service -x "run -q --bin auth"
```
```bash
cargo watch -c -q -w src/health-check-service -x "run -q --bin health-check"
```
## CI/CD Tools

There are the tools used to setup CI/CD.

### Github Actions

[GitHib Actions](https://docs.github.com/en/actions) allow you to execute arbitrary workflows by simply adding a `YAML` file to your repository.

### Docker

[Docker](https://www.docker.com/) is a platform for building, running, and shipping applications in containers. 

Containerization is a technology that allows developers to package an application with all of its dependencies into a standardized unit, called a container, which can be easily deployed across different environments, including local machines, data centers, and cloud providers. Containers are lightweight, portable, and secure, enabling teams to build and deploy applications faster and more reliably.

Docker images are the blueprints or templates used to create Docker containers. An image contains all the necessary files, libraries, and dependencies required to run an application. A container, on the other hand, is a running instance of an image. It's a lightweight, isolated environment that runs the application and its dependencies. Multiple containers can be created from the same image, each with its own unique state and running independently.
