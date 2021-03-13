# udacity-books-capstone

Udacity Books is the capstone project for the Cloud Developer Nanodegree

This projects consists of three microservices:
 - [udacity-books-api](https://github.com/dcollioni/udacity-books-api)
 - [udacity-auth-api](https://github.com/dcollioni/udacity-auth-api)
 - [udacity-books-web](https://github.com/dcollioni/udacity-books-web)

## udacity-books-api
- Node.js REST API that exposes endpoints to create, read, update and delete books
- Connects to a MongoDB database to manage books documents
- Allows the upload of images to AWS S3 through signed URLs
- Identify users using `user-id` request header
- Deployed on AWS EKS behind a `ingress-nginx` configured with authentication
- Travis config to build and push docker image to `dcollioni/udacity-books-api`

* Public URL: https://api.udacity-books.tk/
* Docker image: https://hub.docker.com/r/dcollioni/udacity-books-api

## udacity-auth-api
- Node.js REST API that exposes an endpoint to check user authentication
- Checks authorization header bearer token against auth0 app certificate
- Includes a `user-id` header in the response when authentication is successful
- Deployed on AWS EKS as a ClusterIP service (internal access only)
- Travis config to build and push docker image to `dcollioni/udacity-auth-api`

* Docker image: https://hub.docker.com/r/dcollioni/udacity-auth-api

## udacity-books-web
- React.js SPA that allows users to create, read, update and delete books
- Uploads pictures to AWS S3 through signed URLs
- Allows login through auth0 application
- Makes requests to `udacity-books-api` sending authorization header bearer token
- Deployed on AWS EKS as a LoadBalancer service
- Travis config to build and push docker image to `dcollioni/udacity-books-web`

* Public URL: http://a8f35c57ac9044883a6900b45a50b034-1191308401.eu-west-1.elb.amazonaws.com/
* Docker image: https://hub.docker.com/r/dcollioni/udacity-books-web

# architecture
- Udacity Books works in a microservices architecture
- The main api (https://api.udacity-books.tk/) is mapped to a AWS load balancer that maps requests to a `ingress-nginx`
- All requests received are redirected to the internal service `udacity-auth-api` to check authorization header
- In case of success, the ingress includes a `user-id` header in the request and redirects it to `udacity-books-api`
- In case of unauthorized response, the ingress responds with status 401

# screenshots
- [App](/screenshots/app)
- [DNS](/screenshots/dns)
- [Docker](/screenshots/docker)
- [Kubernetes](/screenshots/kube)
- [MongoDB](/screenshots/mongodb)
- [S3](/screenshots/s3)
- [Travis](/screenshots/travis)