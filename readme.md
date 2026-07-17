# Домашнее задание к занятию «Helm»



## Задание 1. Подготовить Helm-чарт для приложения

### Настройка kubectl для MicroK8s
```
export KUBECONFIG=/var/snap/microk8s/current/credentials/client.config
```

### Создание Helm‑чарта

```
helm create myapp
```
- Удалить ненужные шаблоны (serviceaccount.yaml, hpa.yaml, httproute.yaml), если они не используются.

### Настройка [values.yaml](myapp/values.yaml)

### Генерация TLS‑сертификата
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key -out tls.crt \
  -subj "/CN=myapp.example.com/O=myapp"
```

### Создание TLS‑секрета
```
kubectl create secret tls tls-secret \
  --cert=tls.crt \
  --key=tls.key \
  --namespace=default
```
### Установка Helm‑релиза

```
helm install webapp ./myapp

# или, если релиз уже установлен:

helm upgrade webapp ./myapp
```

### Проверка ресурсов
```
helm list
kubectl get all
kubectl get ingress
kubectl describe ingress webapp-ingress
```
### Перезапуск Traefik Pod (если нужно)
```
kubectl delete pod -n ingress <traefik-pod-name>
```
### Тестирование доступа
```
curl http://myapp.example.com

# или если используется proxy

curl -k --noproxy '*' https://myapp.example.com
```

