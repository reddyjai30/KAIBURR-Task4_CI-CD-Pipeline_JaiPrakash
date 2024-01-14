![1583864622173-removebg-preview](https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/9f5a6975-67cd-4218-99a8-9e7a9312faa1)

# KAIBURR Coding Assignment
##### Name: Jai Prakash Reddy D 
##### R.no: CB.EN.U4CSE20027
###### Mail: reddyjai30@gmail.com



# Task 4 - CI CD Pipeline

## Overview 
Here we will be doing continues Itegration and Deployment from the UI we created using react app using GIT Lab where we make commints and deploy
 
------  


## Git-Lab

Implementation of the yml file for CI-CD implementations where i have done this on the Task 1 Srping boot
<img width="1280" alt="Screenshot 2024-01-14 at 8 51 10 PM" src="https://github.com/reddyjai30/KAIBURR-Task4_CI-CD-Pipeline_JaiPrakash/assets/47852931/505d9598-7d41-4d96-8728-084af464b9bf">

Now Built which is done WRT to Task 3 UI React

`.yml`
```bash


stages:          # List of stages for jobs, and their order of execution
  - build
  - test
  - deploy

build-job:       # This job runs in the build stage, which runs first.
  stage: build
  script:
    - echo "Compiling the code..."
    - echo "Compile complete."

unit-test-job:   # This job runs in the test stage.
  stage: test    # It only starts when the job in the build stage completes successfully.
  script:
    - echo "Running unit tests... This will take about 60 seconds."
    - sleep 60
    - echo "Code coverage is 90%"

lint-test-job:   # This job also runs in the test stage.
  stage: test    # It can run at the same time as unit-test-job (in parallel).
  script:
    - echo "Linting code... This will take about 10 seconds."
    - sleep 10
    - echo "No lint issues found."

deploy-job:      # This job runs in the deploy stage.
  stage: deploy  # It only runs when *both* jobs in the test stage complete successfully.
  environment: production
  script:
    - echo "Deploying application..."
    - echo "Application successfully deployed."
```

`Docker`

```bash
# Stage 1: Build the React application
FROM node:14 as build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . ./
RUN npm run build

# Stage 2: Serve the app with Nginx
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

`docker-compose.yml`
```bash
version: '3.8'
services:
  react-app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:80"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
```
<img width="1000" alt="Screenshot 2024-01-14 at 11 14 40 PM" src="https://github.com/reddyjai30/KAIBURR-Task4_CI-CD-Pipeline_JaiPrakash/assets/47852931/5afb5906-811b-458c-ad40-26d063cefad2">

<img width="1000" alt="Screenshot 2024-01-14 at 11 14 58 PM" src="https://github.com/reddyjai30/KAIBURR-Task4_CI-CD-Pipeline_JaiPrakash/assets/47852931/87f73f86-f2b7-4d11-8c4e-541ad0367c81">


## After commiting the changes the CI CD for the react ui inclusive docker build step in our pipeline
<img width="1000" alt="Screenshot 2024-01-14 at 11 06 04 PM" src="https://github.com/reddyjai30/KAIBURR-Task4_CI-CD-Pipeline_JaiPrakash/assets/47852931/48e497f6-860c-4842-901c-accb1bb846df">

<img width="1000" alt="Screenshot 2024-01-14 at 11 09 23 PM" src="https://github.com/reddyjai30/KAIBURR-Task4_CI-CD-Pipeline_JaiPrakash/assets/47852931/256d967c-08b0-4251-a61d-7c8ab8cfb0c6">



