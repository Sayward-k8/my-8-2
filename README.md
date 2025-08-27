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
	
```
sh
/usr/local/go/bin/go test .
docker build . -t ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER
docker login ubuntu-bionic:8082 -u admin -p admin1 && docker push ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER && docker logout
```

![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/1-1.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/1-2.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/1-3.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/1-4.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/1-5.png)

</details>

### Задание 2

**Что нужно сделать:**

	1. Создайте новый проект pipeline.
	2. Перепишите сборку из задания 1 на declarative в виде кода.

	В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 2

<details>
	
```
pipeline {
 agent any
 stages {
  stage('Git') {
   steps {git branch: 'main', url: 'https://github.com/Sayward-k8/8-2-hw'}
  }
  stage('Test') {
   steps {
    sh 'go test .'
   }
  }
  stage('Build') {
   steps {
    sh 'docker build . -t ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER'
   }
  }
  stage('Push') {
   steps {
    sh 'docker login ubuntu-bionic:8082 -u admin -p admin1 && docker push ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER && docker logout'   }
  }
 }
}

```

![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/2-1.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/2-2.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/2-3.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/2-4.png)
 
</details>

### Задание 3

**Что нужно сделать:**

	1. Установите на машину Nexus.
	2. Создайте raw-hosted репозиторий.
	3. Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
	4. Загрузите файл в репозиторий с помощью jenkins.

	В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 3

<details>

![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/3-1.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/3-2.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/3-3.png)

```
pipeline {
    agent any
    
    environment {
        NEXUS_URL = "http://ubuntu-bionic:8081"
        REPOSITORY = "raw-repo"
        BINARY_NAME = "hello-world"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sayward-k8/8-2-hw'
            }
        }
        
        stage('Build') {
            steps {
                sh 'CGO_ENABLED=0 GOOS=linux go build -a -installsuffix nocgo -o ${BINARY_NAME} .'
            }
        }
        
        stage('Upload to Nexus') {
            steps {
                sh '''
                    curl -u admin:admin1 -X PUT --upload-file ${BINARY_NAME} \
                    "${NEXUS_URL}/repository/${REPOSITORY}/${BINARY_NAME}"
                '''
            }
        }
    }
}
```
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/3-4.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/3-5.png)


</details>

### Задание 4*

	Придумайте способ версионировать приложение, чтобы каждый следующий запуск сборки присваивал имени файла новую версию. Таким образом, в репозитории Nexus будет храниться история релизов.

	Подсказка: используйте переменную BUILD_NUMBER.

	В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 4

<details>

```
pipeline {
    agent any
    
    environment {
        NEXUS_URL = "http://ubuntu-bionic:8081"
        REPOSITORY = "raw-repo"
        BINARY_NAME = "hello-world"
        VERSION = "v${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sayward-k8/8-2-hw'
            }
        }
        
        stage('Build') {
            steps {
                sh 'CGO_ENABLED=0 GOOS=linux go build -a -installsuffix nocgo -o ${BINARY_NAME} .'
            }
        }
        
        stage('Upload to Nexus') {
            steps {
                sh '''
                    curl -u admin:admin1 -X PUT --upload-file ${BINARY_NAME} \
                    "${NEXUS_URL}/repository/${REPOSITORY}/${BINARY_NAME}-${VERSION}"
                '''
            }
        }
    }
}
```

![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/4-2.png)
![alt text](https://github.com/Sayward-k8/my-8-2/blob/main/img/4-3.png)

</details>
