# devdays-eu-2018

### Set Env:
```
export ACCOUNT_ID=$(aws sts get-caller-identity --query "Account" --output text)
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
export DOCKER_LOGIN=`aws ecr get-login --no-include-email`
export PASSWORD=`echo $DOCKER_LOGIN | cut -d' ' -f6`
export REGISTRY=`echo $DOCKER_LOGIN | cut -d' ' -f7 | sed "s/https:\/\///"`
export DOCKER_USER=AWS 
export DOCKER_PASSWORD=${PASSWORD} 
export AWS_DEFAULT_REGION=$AWS_REGION
export CLAIR_ADDR=coreo-Clair-HQCJEAT3VJ0Y-963444812.eu-west-1.elb.amazonaws.com

export ANCHORE_CLI_SSL_VERIFY=n
export ANCHORE_CLI_PASS=foobar
export ANCHORE_CLI_USER=admin
export ANCHORE_CLI_URL=http://localhost:8228/v1

aws configure set default.region ${AWS_REGION}  
$(aws ecr get-login --no-include-email)
aws ecr create-repository --repository-name sample-app
```

### Build Debian Image:
```
docker build . -f Dockerfile-jessie -t ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:jessie
docker push ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:jessie
docker images | grep sample-app | egrep '(jessie)' 
docker run --init --rm -p 8080:8080 -ti 250756795962.dkr.ecr.eu-west-1.amazonaws.com/sample-app:jessie
```

### Scan Debian Image:
```
anchore-cli image add ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:jessie
anchore-cli image wait ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:jessie
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:jessie all
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:jessie all | wc -l
klar ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:jessie
```

### Build Original Centos Image:
```
docker build . -f Dockerfile-centos-original -t ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos-original
docker push ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos-original
docker images | grep sample-app | egrep '(centos-original)' 
docker run --init --rm -p 8080:8080 -ti 250756795962.dkr.ecr.eu-west-1.amazonaws.com/sample-app:centos-original
```

### Build Centos Image:
```
docker build . -f Dockerfile-centos -t ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos
docker push ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos
docker images | grep sample-app | egrep '(centos)' 
docker run --init --rm -p 8080:8080 -ti 250756795962.dkr.ecr.eu-west-1.amazonaws.com/sample-app:centos
```

### Scan Original Centos Image:
```
anchore-cli image add ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos-original
anchore-cli image wait ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos-original
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos-original all
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos-original all | wc -l
klar ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos-original
```

### Scan "Optimized" Centos Image:
```
anchore-cli image add ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos
anchore-cli image wait ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos all
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos all | wc -l
klar ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos
```

### Build Alpine Image:
```
docker build . -f Dockerfile-alpine -t ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:alpine
docker push ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:alpine
docker images | grep sample-app | egrep '(centos|alpine)' 
docker run --init --rm -p 8080:8080 -ti 250756795962.dkr.ecr.eu-west-1.amazonaws.com/sample-app:alpine
```

### Scan Alpine Image:
```
anchore-cli image add ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:alpine
anchore-cli image wait ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:alpine
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:alpine all
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:alpine all | wc -l
klar ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:alpine
```
### Build Distroless Image:
```
docker build . -f Dockerfile-distroless -t ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:distroless
docker push ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:distroless
docker run --init --rm -p 8080:8080 -ti 250756795962.dkr.ecr.eu-west-1.amazonaws.com/sample-app:distroless
```

### Scan Distroless Image:
```
anchore-cli image add ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:distroless
anchore-cli image wait ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:distroless
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:distroless all
anchore-cli image vuln ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:distroless all | wc -l
klar ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:distroless
```

### Compare image sizes:
```
docker images | grep sample-app | egrep '(jessie|centos|alpine|distroless)' 
```

### Explore images:
```
docker run -ti --entrypoint=sh ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:jessie
docker run -ti --entrypoint=sh ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:centos
docker run -ti --entrypoint=sh ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:alpine
docker run -ti --entrypoint=sh ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/sample-app:distroless
```

See also:

https://eksworkshop.com

https://ecsworkshop.com

https://containersonaws.com

https://github.com/anchore/anchore-engine

https://github.com/optiopay/klar

https://github.com/jasonumiker/clair-ecs-fargate

https://github.com/aws-samples/aws-codepipeline-docker-vulnerability-scan

https://github.com/GoogleContainerTools/distroless
