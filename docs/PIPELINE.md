# PIPELINE PROCESS

Inside `.circleci/config.yml` file, we specify the process that will be executed to deploy our application.

## Orbs 

First we define the orbs we are going to use in deployment process:
```
version: 2.1
orbs:
  node: circleci/node@4.2.0
  aws-cli: circleci/aws-cli@2.0.0
  eb: circleci/aws-elastic-beanstalk@2.0.1
```
## Jobs 

- We need to configure aws-cli in order to be able to connect to aws services:

```
jobs:
  conf:
    working_directory: ~/app/
    executor: node/default
    steps:
      - aws-cli/install
      - run:
          name: Configure AWS Access Key ID
          command: aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
      - run:
          name: Configure AWS Secret Access Key
          command: aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
      - run:
          name: Configure AWS default region
          command: aws configure set region $AWS_REGION
```
### Front-End

- Change working directory: 
```
  frontend:
    working_directory: ~/app/udagram-frontend
    executor: node/default
```

- Install packages:
```
    steps:
      - checkout:
          path: ~/app
      - node/install-packages:
          app-dir: ~/app/udagram-frontend
```

- Build frontend:
```
      - run:
          name: Build Frontend
          command: npm run build
```

- Instal aws-cli and deploy frontend: 
```
      - aws-cli/install
      - run:
          name: Deploy Frontend
          command: aws s3 cp --recursive --acl public-read ./www $S3_BUCKET_FRONTEND
```
### Server API

- Change working directory: 
```
  api:
    working_directory: ~/app/udagram-api
    executor: node/default
```

- Install packages:
```
    steps:
      - checkout:
          path: ~/app
      - node/install-packages:
          app-dir: ~/app/udagram-api
```

- Build and Compress Server:
```
      - run:
          name: Build and Compress Server
          command: |-
            npm run build
            npm run zip
```

- Install aws-cli and eb:
```
      - aws-cli/install
      - eb/setup
```

- Deploy server:
```
      - run:
          name: Deploy Server
          command: |-
            eb init udagram-api --platform node.js --region $AWS_REGION
            eb use $EB_ENV
            eb deploy $EB_ENV
```

## Workflows

```
workflows:
  build_and_deploy:
    jobs:
      - conf
      - frontend
      - api

```