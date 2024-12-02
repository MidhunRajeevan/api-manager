# Krakend API Manager

## Installation with Docker

To install Krakend API Manager using Docker, follow these steps:

1. Pull the latest Krakend Docker image by running the following command:

    ```sh
    docker pull devopsfaith/krakend
    ```

2. Create a directory for your Krakend configuration file and place your `krakend.json` file in it.
3. Run the following command to start Krakend with your configuration:

    ```
    sudo docker run -p 8080:8080 -v $PWD:/etc/krakend/ devopsfaith/krakend run --config /etc/krakend/krakend.json
    ```

This command will start Krakend in a Docker container, mapping port 8080 on your host to port 8080 in the container and mounting your configuration file.

## Configuration

To configure Krakend API Manager, you need to create a configuration file in JSON format. Below is an example configuration:

```json
{
  "version": 2,
  "name": "Krakend API Gateway",
  "port": 8080,
  "endpoints": [
     {
        "endpoint": "/api/v1/example",
        "method": "GET",
        "backend": [
          {
             "url_pattern": "/example",
             "host": [
                "http://backend-service"
             ]
          }
        ]
     }
  ]
}
```

Save this configuration file as `krakend.json` and place it in the same directory as the Krakend executable.

To start Krakend with this configuration, run:

```sh
./krakend run -c krakend.json
```

## API-Key and Role Based Access

Krakend supports API-key and role-based access control. To enable this, you need to configure the `auth` section in your `krakend.json` file.

Example configuration:

```json
{
  "version": 2,
  "name": "Krakend API Gateway",
  "port": 8080,
  "endpoints": [
     {
        "endpoint": "/api/v1/secure",
        "method": "GET",
        "backend": [
          {
             "url_pattern": "/secure",
             "host": [
                "http://backend-service"
             ]
          }
        ],
        "extra_config": {
          "auth/validator": {
             "alg": "HS256",
             "jwk-url": "http://auth-service/.well-known/jwks.json",
             "roles_key": "roles",
             "roles": ["admin", "user"]
          }
        }
     }
  ]
}
```

In this example, the `auth/validator` section specifies the JWT algorithm, the URL to fetch the JWKs, and the roles required to access the endpoint.

To use API keys, you can configure the `auth/key-auth` section:

```json
{
  "version": 2,
  "name": "Krakend API Gateway",
  "port": 8080,
  "endpoints": [
     {
        "endpoint": "/api/v1/secure",
        "method": "GET",
        "backend": [
          {
             "url_pattern": "/secure",
             "host": [
                "http://backend-service"
             ]
          }
        ],
        "extra_config": {
          "auth/key-auth": {
             "keys": ["your-api-key-1", "your-api-key-2"]
          }
        }
     }
  ]
}
```

In this example, the `auth/key-auth` section specifies the valid API keys required to access the endpoint.

For more detailed information, refer to the [Krakend documentation](https://www.krakend.io/docs/).
