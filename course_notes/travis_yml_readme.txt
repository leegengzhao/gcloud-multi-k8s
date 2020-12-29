Write .travis.yml. For flow of thing that need to be done inside travis environment, see flow chart slide in course notes.

In the course, we need to install travis cli to encrypt gcloud account json file. Travis cli requires Ruby. In MacOS, ruby is already installed. But for other platform, installing ruby is pain. In the course, we instead install a docker container of ruby, then:

- 'gem install travis' to install travis cli inside the temporary ruby container, 
- login travis 'travis login' with your github id,
- cd to the 'volumed' folder like '/app' that has your service account json file,
- place the service-account.json to the 'volumed' folder,
- run travis cli command 'travis encrypt-file service-account.json -r StephenGrider/multi-k8s' to encrypt the json file to produce service-account.json.enc,
- add the 'openssl ...' to travis.yml file
- git add enc file to git repository and DELETE original service-account.json,
- then we no longer need to retain ruby container.

Separate deploy scripti (custom deploy provider):
deploy:
	provider: script
	script: bash ./deply.sh
	on:
		branch: master

In ./deploy.sh, we will:
- build all images, tag them with latest as well as $GIT_SHA, and push to docker hub
- apply k8s configuration files, 'kubectl apply -f k8s'
- run kubectl to force (imperatively) kubernetes to run $SHA versioned image, like so:
kubectl set image deployments/server-deployment server=stephengrider/multi-server:$SHA

