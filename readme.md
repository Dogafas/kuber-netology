# Домашнее задание к занятию «Настройка приложений и управление доступом в Kubernetes»

## Задание 1. Работа с ConfigMaps

1. Развернуть приложение (nginx + multitool), решить проблему конфигурации через ConfigMap и подключить веб-страницу.

   * (я использовал microk8s, можно выполнить все команды про порядку)

### Применить [ConfigMap](configmap-web.yaml) и [Deployment](deployment.yaml)

```
microk8s kubectl apply -f configmap-web.yaml

microk8s kubectl apply -f deployment.yaml
```

### Проверить, что поды запущены

```
microk8s kubectl get pods

microk8s kubectl describe pod web-app-<id>
```

### Получить IP пода или пробросить порт

```
microk8s kubectl port-forward deploy/web-app 8080:80
```

### Проверить доступность страницы

```
curl http://localhost:8080
```

### Screenshot

![alt text](image.png)

##### Удалить

```
microk8s kubectl delete -f deployment.yaml
microk8s kubectl delete -f configmap-web.yaml
```