# nginx-forwarder
Small Docker image purely for forwarding with 301 HTTP code

Listens to port 80 and responds with a redirect to https://${REDIRECT_TO}, preserving the query.

Relevant usecase: running a site example.com on Google Cloud Run, we need a way to capture www.example.com traffic as well. This cannot be handled on DNS level, nor easily within Google Cloud Run, nor is it good practice to hardcode it in the actual application. Therefore, a for-purpose "application" (this one) must be made.

Since GCR handles SSL, we do not need to worry about that. Listen to http requests, send towards https.

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

To push to your Google Cloud Run environment
````bash
docker pull tacov/forwarder
docker tag tacov/forwarder europe-west4-docker.pkg.dev/punpun-395414/docker/forwarder
docker push europe-west4-docker.pkg.dev/punpun-395414/docker/forwarder
````

# See also
https://hub.docker.com/_/nginx/ --> Using environment variables in nginx configuration