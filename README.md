# build-push-image-tekton
My demo showing how to create, build and push a docker image to DockerHub

## Install Tekton Pipelines
```
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

## Install Tekton Dashboard
```
kubectl apply --filename https://storage.googleapis.com/tekton-releases/dashboard/latest/release.yaml
```

To view, the dashboard
```
kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097
```

<img width="1792" alt="Screenshot 2023-10-12 at 13 24 15" src="https://github.com/joellord/handson-tekton/assets/49791498/ac8e01ca-e00d-4bd3-800f-b534d9cb9721">

## Install Tekton's CLI (MacOS)
```
brew install tektoncd-cli
```

## Authenticating to DockerHub
- Log into Docker via CLI

```
docker login
```

- Create an encoded string of your ```docker-hub-user-name-:docker-hub-password```

```
$ echo -n '<docker-hub-username>:<docker-hub-password>' | base64
ENCODED-STRING
```

- Edit ```~/.docker/config.json```

```
$ sudo nano ~/.docker/config.json
{ "auths": { "https://index.docker.io/v1/": { "auth": "ENCODED-STRING" } } }
```

- Encode ```~/.docker/config.json```

```
$ cat ~/.docker/config.json | base64
ENCODED-JSON-FILE
```

## Create Deployment
- Create a DockerHub repository to store your image in.
```
tkn hub install task git-clone
tkn hub install task kaniko
kubectl apply -f docker-secret.yaml
kubectl apply -f pipeline.yaml
kubectl create -f pipeline-run.yaml
```

<img width="1792" alt="image" src="https://github.com/joellord/handson-tekton/assets/49791498/de4f256b-1765-41b3-ad1f-1f18e7293fb9">

*Task*

<img width="1792" alt="image" src="https://github.com/joellord/handson-tekton/assets/49791498/61579cdf-c1c7-4493-b15d-c0b46c209b8a">

*Pipeline*

<img width="1792" alt="Screenshot 2023-10-12 at 14 57 28" src="https://github.com/joellord/handson-tekton/assets/49791498/a47daaf0-e6fd-43b2-a2c4-8611ec770937">

*PipelineRun*

<img width="1792" alt="Screenshot 2023-10-12 at 15 46 47" src="https://github.com/joellord/handson-tekton/assets/49791498/f6bb5ed1-c9e2-4184-acbe-a12176229b24">

[Article]()