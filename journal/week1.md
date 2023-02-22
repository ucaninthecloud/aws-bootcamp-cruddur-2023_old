# Week 1 — App Containerization

## Reproducing Session ##

I had the opportunity to join the live stream and I was just watching while Andrew was demonstrating containers. My main goal was to pay attention to what he was doing so I could reproduce it later.

I created a new branch and will keep what is needed to merge with main.

I do not use Google Chrome so I can't install the Gitpod plugin. I loaded gitpod with the week1-session branch like this:

```
https://gitpod.io/#https://github.com/ucaninthecloud/aws-bootcamp-cruddur-2023/tree/week1-session
```

Now the loaded session has some files that I pushed to the branch like _reference\gitpod_vars.

Submitting a PR to update week1 instructions

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

Using OpenAPI has helped me understand better how I am using my current scripts to GET data from my On-premises infrastructure