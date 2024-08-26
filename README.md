# Домашнее задание к занятию "Disaster Recovery. FHRP и Keepalived" - Артем Пестроухов

### Задание 1
- Запустите два simple python сервера на своей виртуальной машине на разных портах
- Установите и настройте HAProxy, воспользуйтесь материалами к лекции по [ссылке](2/)
- Настройте балансировку Round-robin на 4 уровне.
- На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

### Решение 1

Конфигурация HAProxy:

```
frontend example
        mode http
        option tcplog
        bind :8088
        default_backend webservers
        use_backend webservers

backend webservers
        mode http
        balance roundrobin
        option tcplog
        #stats realm Haproxy\ statistic
        server s1 127.0.0.1:8998 check
        server s2 127.0.0.1:9999 check
```
![screenshot](https://github.com/ArtemPestro/git-hw/blob/haproxy/img/haproxy-1.png)

---

### Задание 2
- Запустите три simple python сервера на своей виртуальной машине на разных портах
- Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
- HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
- На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.

### Решение 2

Конфигурация HAProxy:

```
frontend example
        mode http
        bind :8088
        acl ACL_example.local hdr(host) -i example.local www.example.local
        #default_backend webservers
        use_backend webservers if ACL_example.local

backend webservers
        mode http
        balance roundrobin
        option tcplog
        option forwardfor
        #stats realm Haproxy\ statistic
        server s1 127.0.0.1:8998 check weight 2
        server s2 127.0.0.1:9999 check weight 3
        server s3 127.0.0.1:1337 check weight 4
```

![screenshot](https://github.com/ArtemPestro/git-hw/blob/haproxy/img/haproxy-2.png)
