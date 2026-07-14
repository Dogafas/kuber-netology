# Задание 1. Volume: обмен данными между контейнерами в поде

1. Создать Deployment приложения, состоящего из двух контейнеров, обменивающихся данными.

   * (я использовал microk8s, можно выполнить все команды про порядку)

### Прменение [манифеста](containers-data-exchange.yaml)

```
microk8s kubectl apply -f containers-data-exchange.yaml
```

### Проверка №1 — описание пода

```
# Получение имя pod

microk8s kubectl get pods
```



```
# Посмотреть описание:

microk8s kubectl describe pod data-exchange-<id>
```

### Проверка №2 — чтение файла из multitool

```
# Подключиться в контейнер reader:

microk8s kubectl exec -it deploy/data-exchange -c reader -- sh

# Внутри контейнера выполнить:

tail -f /shared/data.txt
```

### Удаление ресурсов

```
# удалить Deployment

microk8s kubectl delete -f containers-data-exchange.yaml
microk8s kubectl delete pod -l app=data-exchange --force --grace-period=0
microk8s kubectl get pods
```
### Screenshots

![alt text](image.png)
скриншот продложение ....
![alt text](image-1.png)