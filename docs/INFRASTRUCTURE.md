# INFRASTRUCTURE DESCRIPTION

This project is hosted on various Amazon web services that are interchangeably connected to serve the complete full-stack web application. The project is also build to fulfil CI/CD (continuous integration and continuous delivery/deployment) best practices.

## Infrastructure components

1. Simple Storage Service S3

This service is used to host files and can be used to host static web pages. In this project, S3 hosts the front-end of the full-stack appilication.

2. Elastic Beanstalk EB

EB is a service for deploying and running web applications. In this project, EB runs the Node.js API that connects with both front-end and database.

3. Relational Database Service RDS

Amazon RDS operates relational databases in the cloud. In this project, RDS operates Postgres SQL database.

4. CircleCi

CircleCi provides automated continuous integration and delivery by running a series of instructions every time a change occurs in the projects repository on GitHub.

5. GitHub

GitHub stores the project source code and keeps track of changes. GitHub invokes CircleCi deployment process at every new commit.