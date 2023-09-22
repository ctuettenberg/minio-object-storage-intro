---
marp: true
title: Short and sweet intro to MinIO object storage
description: Short and sweet intro to MinIO object storage
theme: uncover
paginate: true
_paginate: false

---

# Short and sweet

## Intro to MinIO object storage

---

# Intro, reason & background

#### MinIO is a high-performance, S3 compatible object store

<!--

* implemented in Go language
* built for large scale AI/ML, data lake and database workloads. 
* runs on any cloud or on-premises infrastructure.

-->

[MinIO website](https://min.io/)

[MinIO GitHub](https://github.com/minio/minio)

---

# Features

#### Active Active Replication for Object Storage
![Active-Active Cross Region/Zone Replication](/images/active-active-replication.svg "Active-Active Cross Region/Zone Replication")

#### AWS S3 Compatible Object Storage
![AWS S3 Compatible Object Storage](/images/s3-compatibility.svg "AWS S3 Compatible Object Storage")

#### Automated Data Management Interfaces
![Automated Data Management Interfaces](/images/minio_console.png "Automated Data Management Interfaces")

* Encryption
* Bucket & Object Immutability
* Data Life Cycle Management & Tiering
* Identity & Access Management
* Scalable Object Storage
* Kubernetes-native

<!--

Active Active Replication for Object Storage
Active-Active Cross Region/Zone Replication
MinIO is the only vendor that offers it today

Encryption
MinIO encrypts data when stored on disk and when transmitted over the network. 
MinIO’s state-of-the-art encryption schemes support granular object-level encryption using modern, industry-standard encryption algorithms
Object encryption, network encryption, key encryption service

Automated Data Management Interfaces
MinIO offers a suite of options to cover every persona in a data-driven enterprise, 
such as graphical user interfaces (GUI), command line interfaces (CLI) and application programming interfaces (API)

Bucket & Object Immutability
MinIO supports a complete range of functionality including object locking, retention, legal holds, governance, and compliance.

AWS S3 Compatible Object Storage
MinIO’s S3 implementation is (one of) the most widely tested and implemented alternative to AWS S3 in the world.
One of the earliest adopters of the S3 API (both V2 and V4) and one of the few storage companies to focus exclusively on S3

Data Life Cycle Management
MinIO offers a unique suite of features (versioning, object locking, etc.) to protect data within and across clouds.

Identity & Access Management
MinIO IAM is built with AWS Identity and Access Management (IAM) compatibility at its core 
and presents that framework to applications and users no matter the environment
MinIO also supports leading third-party external identity providers (IDP) - Keycloak, ActiveDirectory, LDAP, etc

Scalable Object Storage
MinIO scales horizontally (scale out) through a concept called Server Pools. 
Each server pool is an independent set of nodes with their own compute, network and storage resources.

Kubernetes-native
With a native Kubernetes operator integration, 
MinIO supports all the major Kubernetes distributions on public, private and edge clouds.

-->

---

# Use case 1

## Alternative to S3 cloud storage in production environments

---

# Use case 2

## Containerized S3 compatible storage for local development

Easy usage with docker-compose - no additional requirements

```
version: '3.8'
services:
  s3:
    image: minio/minio
    ports:
      - "9000:9000" # API
      - "9001:9001" # UI console
    volumes:
      - minio_storage:/data
    environment:
      MINIO_ROOT_USER: minio
      MINIO_ROOT_PASSWORD: BaD#PaSsWoRD#ChAnGeMe
    command: server --console-address ":9001" /data

volumes:
  minio_storage: {}
```
Docker image
[Docker Hub - minio/minio](https://hub.docker.com/r/minio/minio/tags)

---

Create default MinIO client (Java)

* endpoint
* accesKey / user
* secretKey / password

```
import io.minio.MinioClient;

MinioClient minioClient = MinioClient.builder()
    .endpoint("https://localhost:9000")
    .credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
    .build();
```

---

Create MinIO client with AWS S3 SDK (Java)
* accesKey / user
* secretKey / password
* endpoint
* region (set to null, if not configured in UI console)
* pathStyleAccessEnabled = true (required for MinIO)

```
import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.auth.BasicAWSCredentials;
import com.amazonaws.client.builder.AwsClientBuilder.EndpointConfiguration;
import com.amazonaws.services.s3.AmazonS3;
import com.amazonaws.services.s3.AmazonS3ClientBuilder;

BasicAWSCredentials credentials =
    new BasicAWSCredentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG");
EndpointConfiguration endpointConfig =
    new EndpointConfiguration("https://localhost:9000", null);
AmazonS3 amazonS3 = AmazonS3ClientBuilder.standard()
    .withCredentials(new AWSStaticCredentialsProvider(credentials))
    .withEndpointConfiguration(endpointConfig)
    .withPathStyleAccessEnabled(true)
    .build();
```

---

# Demo - Travel Expenses Tool

---

# Q&A