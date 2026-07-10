# Задание 1. Создать Deployment и обеспечить доступ к репликам приложения из другого Pod

1. Создать Deployment и обеспечить доступ к контейнерам приложения по разным портам из другого Pod внутри кластера

    * (я использовал microk8s, можно выполнить все команды про порядку)

### Запуск [Deployment](deployment-task3.yaml)

```
microk8s kubectl apply -f deployment-task3.yaml
```

### Запуск [Service](service-task3.yaml)

```
microk8s kubectl apply -f service-task3.yaml
```

### Запуск [Pod](multitool-pod.yaml)

```
microk8s kubectl apply -f multitool-pod.yaml
```

### Проверки

```
# 3 Pod с двумя контейнерами + Pod multitool‑client

microk8s kubectl get pods
```

```
# сервис с портами 9001 и 9002.

microk8s kubectl get svc
```

```
# страница nginx на 9001 порту ⤵

microk8s kubectl exec -it multitool-client -- curl nginx-svc-task3:9001

# страница multitool на 9002 порту ⤵

microk8s kubectl exec -it multitool-client -- curl nginx-svc-task3:9002
```

### Удаление

```
microk8s kubectl delete deployments,pods --all
```

#### screenshot №1

![alt text](image.png))

#### screenshot №2

![alt text](image-1.png)
