# HAProxy Load Balancing Apache Backends Stack

This stack demonstrates how HAProxy can load balance multiple backends of Apache Web server.

## Running the Stack

When you run the stack, you can load the HAProxy frontend at http://mesh-leader:8886 (where `mesh-leader` is the hostname or IP address of the host that is running that container).

Also, you can inspect the HAProxy stats by accessing http://mesh-leader:1936/haproxy?stats. Basic Authentication is used. When prompted, enter `occsdemo` as the username and `occspass` as the password.

## Apache Backend

The Apache backend image includes a default page that prints the hostname of the container. This allows you to refresh the HAProxy frontend and see the contents of the Web page change when a different backend serves up the content.

## HAProxy

HAProxy load balances between a dynamic number of Apache backends. In order to do this, the HAProxy container must derive the IP addresses and ports of all the Apache backends. This process requires the following environment variables to be defined:

* `OCCS_API_TOKEN={{api_token}}` - the token for authenticating against the key/value endpoint
* `KV_IP=172.17.0.1` - the IP address which provides the key/value endpoint, in this case the host running the container
* `KV_PORT=9109` - the port on which the key/value endpoint is listening
* `OCCS_BACKEND_KEY={{sd_deployment_containers_path "backend" 80}}` - the key prefix in the key/value store that can be used to lookup the IP and published port of each backend
* `OCCS_HEALTHCHECK_HTTP=http://:8886/?timeout=10s&interval=30s` - the healthcheck URL to use
