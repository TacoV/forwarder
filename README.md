# nginx-forwarder
Small Docker image purely for forwarding with 301 HTTP code

Listens to ${REDIRECT_FROM}:80 and responds with a redirect to https://${REDIRECT_TO}, preserving the query.
