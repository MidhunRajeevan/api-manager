# Kong API Manager

## Installation

To install Kong using Docker, you can use the `kong.yaml` file. Run the following command to start Kong:

```sh
docker-compose -f kong.yaml up -d
```

## Verifying Kong and Konga

To verify that Kong and Konga are running correctly, you can use the following steps:

1. **Verify Kong**:
    - Open your browser and navigate to [http://localhost:8001](http://localhost:8001). You should see the Kong Admin API.
    - To verify the proxy, navigate to [http://localhost:8000](http://localhost:8000). You should see a response indicating that Kong is running.

2. **Verify Konga**:
    - Open your browser and navigate to [http://localhost:1337](http://localhost:1337). You should see the Konga login page.

If you encounter any issues, check the Docker container logs for more information.

## Configuring Konga

Konga is a GUI for managing Kong. You can access Konga at [http://localhost:1337/](http://localhost:1337/). To connect Konga to Kong, use the Kong Admin URL: [http://kong:8001](http://kong:8001).

## Creating a Service

To create a new service in Kong, use the following `curl` command:

```sh
curl -i -s -X POST http://localhost:8001/services \
 --data name=users \
 --data url='http://your-service-url'
```

## Creating Routes

To create routes for the service, use the following `curl` command:

```sh
curl -i -X POST http://localhost:8001/services/users/routes \
 --data 'paths[]=/krakend-api/users/id' \
 --data name=users-id \
 --data "methods[]=GET" \
 --data "methods[]=PATCH" \
 --data "service.name=users"
```

This will create routes for the `users` service with the specified paths and methods.
