# Домашнее задание к занятию "Введение в DevOps: Что такое DevOps. СI/СD" - Артем Пестроухов

---

### Задание 1

**Что нужно сделать:**

1. Установите себе jenkins по инструкции из лекции или любым другим способом из официальной документации. Использовать Docker в этом задании нежелательно.
2. Установите на машину с jenkins [golang](https://golang.org/doc/install).
3. Используя свой аккаунт на GitHub, сделайте себе форк [репозитория](https://github.com/netology-code/sdvps-materials.git). В этом же репозитории находится [дополнительный материал для выполнения ДЗ](https://github.com/netology-code/sdvps-materials/blob/main/CICD/8.2-hw.md).
3. Создайте в jenkins Freestyle Project, подключите получившийся репозиторий к нему и произведите запуск тестов и сборку проекта ```go test .``` и  ```docker build .```.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 1

![скрин_1](https://github.com/ArtemPestro/git-hw/blob/devops-cicd/img/cicd-1.1.png)

![скрин_3](https://github.com/ArtemPestro/git-hw/blob/devops-cicd/img/cicd-1.3.png)

---

### Задание 2

**Что нужно сделать:**

1. Создайте новый проект pipeline.
2. Перепишите сборку из задания 1 на declarative в виде кода.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 2

![Вывод Jenkins об успешной сборке](https://github.com/ArtemPestro/git-hw/blob/devops-cicd/img/cicd-2.png)

Pipeline-код Jenkins:
```
pipeline {
 agent any
 stages {
  stage('Git') {
   steps {git 'https://github.com/ArtemPestro/ci-cd'}
  }
  stage('Test') {
   steps {sh 'go test .'}
  }
  stage('Build') {
   steps {sh 'docker build -t artempestro/ci-cd-homework:$BUILD_NUMBER .'}
  }
  stage('Push') {
   steps {sh 'docker login -u artempestro -p && docker push artempestro/ci-cd-homework:$BUILD_NUMBER && docker logout'}
  }
 }
}
```

---

### Задание 3

**Что нужно сделать:**

1. Установите на машину Nexus.
1. Создайте raw-hosted репозиторий.
1. Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
1. Загрузите файл в репозиторий с помощью jenkins.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

### Решение 3

Pipeline:

```
pipeline {
 agent any
 stages {
  stage('Git') {
   steps {git 'https://github.com/ArtemPestro/ci-cd'}
  }
  stage('Test') {
   steps {
    sh 'go test .'
   }
  }
  stage('Build') {
   steps {
    sh 'go build -a -installsuffix nocgo'
   }
  }
  stage('Push') {
   steps {
    sh 'curl -u admin: http://127.0.0.1:8081/repository/ci-cd-homework-3/ --upload-file ./sdvps-materials'   }
  }
 }
}
```

![скрин-3.1](https://github.com/ArtemPestro/git-hw/blob/devops-cicd/img/cicd-3.png)

---
## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 4*

Придумайте способ версионировать приложение, чтобы каждый следующий запуск сборки присваивал имени файла новую версию. Таким образом, в репозитории Nexus будет храниться история релизов.

Подсказка: используйте переменную BUILD_NUMBER.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.
