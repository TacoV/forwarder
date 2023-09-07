# forwarder
Small Docker image purely for forwarding with [HTTP 301](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/301) code.
Published on Docker Hub as [tacov/forwarder](https://hub.docker.com/r/tacov/forwarder).

Listens to port 80 and responds with a redirect to https://${REDIRECT_TO}, preserving the query.

Relevant usecase: running a site example.com on Google Cloud Run, we need a way to capture www.example.com traffic as well. This cannot be handled on DNS level, nor easily within Google Cloud Run, nor is it good practice to hardcode it in the actual application. Therefore, a for-purpose "application" must be used. Since GCR handles SSL, we do not need to worry about that. Listen to internal http requests, send to https.

# How to use
To build and run locally:
````bash 
docker build . -t my-forwarder
docker run -e REDIRECT_TO=example.com -p80:80 my-forwarder
````

To just use on your machine
````bash
docker run -e REDIRECT_TO=example.com -p80:80 tacov/forwarder
````

or in docker-compose.yml
````yaml
services:
  my-forwarder:
    image: tacov/forwarder
    ports:
      - 80:80
    environment:
      REDIRECT_TO: example.com
````

To push to your Google Cloud Run environment (see also [their docs](https://cloud.google.com/artifact-registry/docs/docker/pushing-and-pulling))
````bash
docker pull tacov/forwarder
docker tag tacov/forwarder [LOCATION]-docker.pkg.dev/[PROJECT-ID]/[REPOSITORY]/forwarder
docker push [LOCATION]-docker.pkg.dev/[PROJECT-ID]/[REPOSITORY]/forwarder
````

# See also
https://hub.docker.com/_/nginx/ --> Using environment variables in nginx configuration
