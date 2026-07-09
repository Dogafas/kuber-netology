# Задание 1. Создать Deployment и обеспечить доступ к репликам приложения из другого Pod

## 1. Создание Deployment
Файл: [deployment.yaml](deployment.yaml)

## 2. Создание Service
Файл: [service.yaml](service.yaml)

## 3. Создание Pod
Файл: [multitool-pod.yaml](multitool-pod.yaml)


Создаем Deployment, проверяем Pod, масштабируем Deployment до 2 шт, создаем Service, создаём Pod multitool-client,.... 

![alt text](image.png)

...(pod netology-deploy-7b766fd89c-gp46b скачался и запустился не сразу...)... проверяем доступ из multitool-client

![alt text](image-1.png)

P.S. был создан Deployment с двумя контейнерами, масштабирован до двух реплик, подключён через Service и успешно проверен доступ к обоим контейнерам из отдельного Pod multitool

# Задание 2. Создать Deployment и обеспечить старт основного контейнера при выполнении условий

Файлы:
- [deployment-task2.yaml](deployment-task2.yaml)
- [service-task2.yaml](service-task2.yaml)

[Deployment](deployment-task2.yaml) с Init‑контейнером блокировал запуск nginx до появления [Service](service-task2.yaml). После создания Service Init завершился, и nginx успешно запустился.

![alt text](image-2.png)
