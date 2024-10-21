# Checkov demo

## Install Hadolint

https://github.com/bridgecrewio/checkov#installation

## Example

> [!NOTE]
> Dockerfile примера взят из официальной документации Checkov https://www.checkov.io/7.Scan%20Examples/Dockerfile.html

Запустим проверку

```
$ checkov -f checkov/Dockerfile --framework dockerfile
```

Результаты
```
[ dockerfile framework ]: 100%|████████████████████|[1/1], Current File Scanned=Dockerfile

       _               _              
   ___| |__   ___  ___| | _______   __
  / __| '_ \ / _ \/ __| |/ / _ \ \ / /
 | (__| | | |  __/ (__|   < (_) \ V / 
  \___|_| |_|\___|\___|_|\_\___/ \_/  
                                      
By Prisma Cloud | version: 3.2.267 

dockerfile scan results:

Passed checks: 23, Failed checks: 2, Skipped checks: 0

. . .

Check: CKV_DOCKER_1: "Ensure port 22 is not exposed"
	FAILED for resource: Dockerfile.EXPOSE
	File: Dockerfile:6-6
	Guide: https://docs.prismacloud.io/en/enterprise-edition/policy-reference/docker-policies/docker-policy-index/ensure-port-22-is-not-exposed

		6 | EXPOSE 3000 22

Check: CKV_DOCKER_3: "Ensure that a user for the container has been created"
	FAILED for resource: Dockerfile.
	File: Dockerfile:1-8
	Guide: https://docs.prismacloud.io/en/enterprise-edition/policy-reference/docker-policies/docker-policy-index/ensure-that-a-user-for-the-container-has-been-created

		1 | FROM node:alpine
		2 | WORKDIR /usr/src/app
		3 | COPY package*.json ./
		4 | RUN npm install
		5 | COPY . .
		6 | EXPOSE 3000 22
		7 | HEALTHCHECK CMD curl --fail http://localhost:3000 || exit 1
		8 | CMD ["node","app.js"]

```
