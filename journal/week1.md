# Week 1 — App Containerization

## Reproducing Session ##

I joined the live session to follow along with the demo and conversation.

I created a new branch and kept all my activities in that one. At the end of the required activities, I merged everything to main.

For anyone that is not using Google Chrome and the Gitpod plugin, you can create a workspace out of a branch by adding */tree/name-of-branch* at the end of the standard URL:

```
https://gitpod.io/#https://github.com/ucaninthecloud/aws-bootcamp-cruddur-2023/tree/week1-session
```

After creating the Dockerfile for both (the backend and frontend containers) I initiated a docker compose. There is always an error when starting the app from a new Gitpod Workspace due to the missing npm install in the frontend directory so I updated  my .gitpod.yml to include the npm install in the /workspace/aws-bootcamp-cruddr-2023/frontend-react-js:

```yml
  - name: npm front-end
    init: |
      cd /workspace/aws-bootcamp-cruddur-2023/frontend-react-js
      npm install
```

I stopped and deleted the Workspace and tried once more. It was failing with a directory not found. On my second attempt I left it with ./frontend-react-js and it worked:

```yml
  - name: npm front-end
    init: |
      cd ./frontend-react-js
      npm install
```

I tried to capture the error by trying again with the complete path but it worked this time. One of those strange gitpod "glitches".




Below is the code that I submited a PR for. I tried to submit early in the week but I missed a couple steps after my fork was created.

```sh
docker run --rm -p 4567:4567 -it backend-flask
FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
unset FRONTEND_URL
unset BACKEND_URL
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
```

For some reason, I still don't trust Gitpod 100% to store my AWS secret access key so, I created a new _reference directory in my repo to place all my *pre-requisites*



After this, compose up will serve the app in Port 3000 without running npm install every time I need to destroy and recreate the workspace.

## My Journey to the Cloud!

I am going to become a **Cloud Platform Engineer**

I am a good fit because **I am interested in enabling developers to get the infrastructure they need in a better and repeatable way via a self-service approach.**

| I will know | I will not get distracted by |
| ---- | ---- |
| Python | Web Development |
| API integrations | Database management |
| Ansible and Terraform | AR, AI or ML |
| Github Actions | Jenkins |
| AWS and Azure | GCP |


