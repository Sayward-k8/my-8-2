# Домашнее задание к занятию "`Что такое DevOps. СI/СD`" - `Игонин В.А.`

### Задание 1

**Что нужно сделать:**

1. Установите себе jenkins по инструкции из лекции или любым другим способом из официальной документации. Использовать Docker в этом задании нежелательно.
2. Установите на машину с jenkins [golang](https://golang.org/doc/install).
3. Используя свой аккаунт на GitHub, сделайте себе форк [репозитория](https://github.com/netology-code/sdvps-materials.git). В этом же репозитории находится [дополнительный материал для выполнения ДЗ](https://github.com/netology-code/sdvps-materials/blob/main/CICD/8.2-hw.md).
3. Создайте в jenkins Freestyle Project, подключите получившийся репозиторий к нему и произведите запуск тестов и сборку проекта ```go test .``` и  ```docker build .```.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 1

<details> 
	
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/1-1.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/1-2.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/1-3.png)
  	 
<details>
	
	Started by user admin
	Running as SYSTEM
	Building in workspace /var/lib/jenkins/workspace/my-pipe2
	The recommended git tool is: NONE
	No credentials specified
	 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/my-pipe2/.git # timeout=10
	Fetching changes from the remote Git repository
	 > git config remote.origin.url https://github.com/Sayward-k8/sdvps-materials # timeout=10
	Fetching upstream changes from https://github.com/Sayward-k8/sdvps-materials
	 > git --version # timeout=10
	 > git --version # 'git version 2.43.0'
	 > git fetch --tags --force --progress -- https://github.com/Sayward-k8/sdvps-materials +refs/heads/*:refs/remotes/origin/* # timeout=10
	 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
	Checking out Revision 223dbc3f489784448004e020f2ef224f17a7b06d (refs/remotes/origin/main)
	 > git config core.sparsecheckout # timeout=10
	 > git checkout -f 223dbc3f489784448004e020f2ef224f17a7b06d # timeout=10
	Commit message: "Update README.md"
	 > git rev-list --no-walk 223dbc3f489784448004e020f2ef224f17a7b06d # timeout=10
	[my-pipe2] $ /bin/sh -xe /tmp/jenkins12395962510819315908.sh
	+ /usr/local/go/bin/go test .
	ok  	github.com/netology-code/sdvps-materials	(cached)
	+ docker build . -t ubuntu-bionic:8082/hello-world:v9
	 
	  #0 building with "default" instance using docker driver
	  
	  #1 [internal] load build definition from Dockerfile
	  #1 transferring dockerfile: 350B done
	  #1 DONE 0.0s
	  
	  #2 [internal] load metadata for docker.io/library/golang:1.16
	  #2 ...
	  
	  #3 [internal] load metadata for docker.io/library/alpine:latest
	  #3 DONE 0.7s
	  
	  #2 [internal] load metadata for docker.io/library/golang:1.16
	  #2 DONE 0.7s
	  
	  #4 [internal] load .dockerignore
	  #4 transferring context: 2B done
	  #4 DONE 0.0s
	  
	  #5 [builder 1/4] FROM docker.io/library/golang:1.16@sha256:5f6a4662de3efc6d6bb812d02e9de3d8698eea16b8eb7281f03e6f3e8383018e
	  #5 DONE 0.0s
	  
	  #6 [stage-1 1/3] FROM docker.io/library/alpine:latest@sha256:4bcff63911fcb4448bd4fdacec207030997caf25e9bea4045fa6c8c44de311d1
	  #6 DONE 0.0s
	  
	  #7 [internal] load build context
	  #7 transferring context: 13.24kB 0.0s done
	  #7 DONE 0.0s
	  
	  #8 [builder 3/4] COPY . ./
	  #8 CACHED
	  
	  #9 [stage-1 2/3] RUN apk -U add ca-certificates
	  #9 CACHED
	  
	  #10 [builder 2/4] WORKDIR /go/src/github.com/netology-code/sdvps-materials
	  #10 CACHED
	  
	  #11 [builder 4/4] RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix nocgo -o /app .
	  #11 CACHED
	  
	  #12 [stage-1 3/3] COPY --from=builder /app /app
	  #12 CACHED
	  
	  #13 exporting to image
	  #13 exporting layers done
	  #13 writing image sha256:3482c6f489bb6f2f8c628f96ef3659aeab57090857297331ec341199812370d2 done
	  #13 naming to ubuntu-bionic:8082/hello-world:v9 done
	  #13 DONE 0.0s
	  + docker login ubuntu-bionic:8082 -u admin -p admin1
	  WARNING! Using --password via the CLI is insecure. Use --password-stdin.
	  
	  WARNING! Your credentials are stored unencrypted in '/var/lib/jenkins/.docker/config.json'.
	  Configure a credential helper to remove this warning. See
	  https://docs.docker.com/go/credential-store/
	  
	  Login Succeeded
	  + docker push ubuntu-bionic:8082/hello-world:v9
	  The push refers to repository [ubuntu-bionic:8082/hello-world]
	  fda926094d5c: Preparing
	  292a8e9ae6de: Preparing
	  418dccb7d85a: Preparing
	  fda926094d5c: Pushed
	  292a8e9ae6de: Pushed
	  418dccb7d85a: Pushed
	  v9: digest: sha256:d32b99accc4e2453999bee5bfa1755a1b34018458f2afb1f4e39718ba8fdee87 size: 950
	  + docker logout
	  Removing login credentials for https://index.docker.io/v1/
	  Finished: SUCCESS   

</details>

</details>

### Задание 2

**Что нужно сделать:**

1. Создайте новый проект pipeline.
2. Перепишите сборку из задания 1 на declarative в виде кода.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 2

### Задание 3

**Что нужно сделать:**

1. Установите на машину Nexus.
1. Создайте raw-hosted репозиторий.
1. Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
1. Загрузите файл в репозиторий с помощью jenkins.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 3

### Задание 4*

Придумайте способ версионировать приложение, чтобы каждый следующий запуск сборки присваивал имени файла новую версию. Таким образом, в репозитории Nexus будет храниться история релизов.

Подсказка: используйте переменную BUILD_NUMBER.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 4
