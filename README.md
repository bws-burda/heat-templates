# Heat template examples

This repository is a collection of heat templates for certain use cases. Copy the <example>-env.dist.yaml
to <example>-env.yaml and add your values. Then create the stack with

```shell
openstack create stack -e <example>-env.yaml -t <example>.yaml <example-stack> 
```

## docker.yaml

This template shows how to create a private network for a server with a loadbalancer that
gets assigned an existing floating ip address.

The server is created with an image that already has Docker installed. 

The server uses cloud-config to run an image.

The loadbalancer has a rule to redirect http requests for a host to the container.

### Prerequistes

1. Create a key pair.
2. Create a floating ip.
3. Create a DNS record for the floating ip.