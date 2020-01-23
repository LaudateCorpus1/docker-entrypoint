# docker-entrypoint
Python script to verify required variables before continuing to startup for Drone plugins.

## Description 
This script can be used as a `ENTRYPOINT` for a Drone plugin to verify the required variables
for the plugin to work. It will also remove the make avaialbe the variable without the
`PLUGIN_` prefix. There are three different types of variables:

* `required_vars` All variables listed here are required. If not provided it will exit with `1`.

* `optional_vars` Any variable that is not required, but may be needed for debug purposes.

* `secret_vars` Variables listed in the `required_vars` or `optional_vars` list that won't be printed when `DEBUG` is set to `True`.

The only Python requirement is the `yaml` module. 

## Example Dockerfile

```
FROM alpine:latest

# Install required packages
RUN apk --no-cache add python py-yaml

COPY entrypoint.yaml /
COPY docker-entrypoint.py /
COPY startup.sh /

RUN chmod +x /startup.sh
RUN chmod +x /docker-entrypoint.py

ENTRYPOINT ["/docker-entrypoint.py"]

CMD ["/startup.sh"]
```
