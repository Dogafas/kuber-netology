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

## Задание 2. Настройка HTTPS с Secrets

1. Развернуть приложение с доступом по HTTPS, используя самоподписанный сертификат.

### Создание [ConfigMap](configmap-web.yaml)

```
microk8s kubectl apply -f configmap-web.yaml
```

### Запуск [Deployment](deployment.yaml)

```
microk8s kubectl apply -f deployment.yaml

microk8s kubectl get pods -o wide
```

### Создание [Service](web-app-service.yaml)

```
microk8s kubectl apply -f web-app-service.yaml

microk8s kubectl get svc web-app-service

microk8s kubectl get endpoints web-app-service
```

### Генерация самоподписанного сертификата

* Файлы tls.crt и tls.key сохраняются в репозитории.

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key -out tls.crt \
  -subj "/CN=myapp.example.com/O=myapp"
```

### Создание [TLS‑секрета](secret-tls.yaml)

```
microk8s kubectl create secret tls tls-secret \
  --cert=tls.crt --key=tls.key
```

* или через манифест

```
microk8s kubectl apply -f secret-tls.yaml
```

### Применение [Ingress](ingress-tls.yaml)

* HTTP‑вариант:

```
microk8s kubectl apply -f ingress-http.yaml
```

* TLS‑вариант:

```
microk8s kubectl apply -f ingress-tls.yaml
```

### Проверка доступа (использую proxy, поэтому добавлял --noproxy '*')

```
curl --noproxy '*' http://myapp.example.com:32094

curl --noproxy '*' -k https://myapp.example.com:32710
```

### Screenshot

![alt text](image-1.png)

### Удаление ресурсов

```
microk8s kubectl delete -f ingress-tls.yaml
microk8s kubectl delete -f ingress-http.yaml
microk8s kubectl delete -f secret-tls.yaml
microk8s kubectl delete -f web-app-service.yaml
microk8s kubectl delete -f deployment.yaml
microk8s kubectl delete -f configmap-web.yaml
```

## Задание 3. Настройка RBAC

1. Создать пользователя с ограниченными правами (только просмотр логов и описания подов).

###
