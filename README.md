# NEONE Server Setup

Welcome to the NEONE Server Setup, in this document you will find the instructions to run a NE:ONE server using your own notification endpoint.

The [neone-make repository](https://github.com/astein-iamovers/neone-make) is an alternative configuration allowing you to forward notifications to the automation platform Make, using Make webhooks. This can also be done with Zappier webhooks. These platforms give you the flexibility to process the notification and implement workflows according to your needs.

## Notifications

ONE Record requires each server to implement a notifications endpoint to receive notifications from other ONE Record servers. The NEONE server already includes this. However, processing the notification and updating your systems is outside the ONE Record scope, as each company needs the flexibility to implement its own rules.
The NEONE Server stores notifications as objects and allows you to forward them to your own custom NOTIFICATION_ENDPOINT. In the current version, NEONE expects the notification endpoint to end with "/notifications". When creating your endpoint, please ensure this structure is followed.

Make or Zappier's webhooks do not contain this "/notifications" suffix so a workaround is required, using a proxy service, Caddy, to receive these notifications and forward them to the Make Webhook.

Follow the instructions on the neone-make repository if you'd like to process notifications with Make.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed
- [Git](https://git-scm.com/downloads) installed
- Have your variables handy, including the NOTIFICATION_ENDPOINT

## Step by step guide

1) Clone this repository
   ```bash
   git clone https://github.com/astein-iamovers/neone.git
   ```
2) Switch to the directory neone
   ```bash
   cd neone
   ```
3) If you have Mac or Linux, please reset folder permissions to allow cross-container dependencies.
   ```bash
   chmod -R 755 ./
   ```
4) Modify your .env file with the nano editor.
   ```bash
   nano .env
   ```
   Paste your variables
   - BASE_1R_HOST:
   - CLIENT_ID: given by IAM
   - CLIENT_SECRET: given by IAM
   - AUDIENCE: given by IAM
   - NOTIFICATION_ENDPOINT:
   
   To save an exit: ctrl+x then Y then Enter
5) Start all services with [docker compose](https://docs.docker.com/compose/)
   ```bash
   sudo docker compose up -d
   ```
6) Wait until all containers are up and running:
   ```bash
   [+] Running 6/6
    ✔ Network docker-compose_default            Created
    ✔ Container docker-compose-graph-db-1       Healthy
    ✔ Container docker-compose-graph-db-setup-1 Started
    ✔ Container docker-compose-ne-one-server-1  Healty
    ✔ Container neone-ne-one-play-1             Started
    ✔ Container neone-ne-one-view-1             Started
        
    
   ```
7) Try to access the ONE Record Server by http://{BASE_1R_HOST}:8080 using your favorite browser. 
   You should see a HTTP Error 401, because you did not authenticate yet. But this confirms that the ONE Record Server is up and running.

# Overview of services

| Name | Description | Base URL / Admin UI |
|-|-|-|
| ne-one-server | [ne-one server](https://git.openlogisticsfoundation.org/wg-digitalaircargo/ne-one) | http://{BASE_1R_HOST}:8080 |
| ne-one-view | [ne-one view](https://git.openlogisticsfoundation.org/wg-digitalaircargo/ne-one-view) | http://{BASE_1R_HOST}:3000 |
| ne-one-play | [ne-one play](https://github.com/aloccid-iata/neoneplay) | http://{BASE_1R_HOST}:3001 |
| graphdb | GraphDB database as database backend for ne-one-server-1 repository neone | http://{BASE_1R_HOST}:7200 |


