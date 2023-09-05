# nginx-forwarder
Small Docker image purely for forwarding with 301 HTTP code

Listens to port 80 and responds with a redirect to https://${REDIRECT_TO}, preserving the query.

# How to use
Set REDIRECT_TO to your preferred domain, eg "example.com"

Relevant usecase: running a site example.com on Google Cloud Run, we need a way to capture www.example.com traffic as well. This cannot be handled on DNS level, nor easily within Google Cloud Run, nor is it good practice to hardcode it in the actual application. Therefore, a for-purpose "application" (this one) must be made.

Since GCR handles SSL, we do not need to worry about that. Listen to http requests, send towards https.

# See also
https://hub.docker.com/_/nginx/ --> Using environment variables in nginx configuration
