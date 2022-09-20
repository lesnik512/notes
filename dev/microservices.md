## Definition of Done

1. Логгирование в stdout - обязательно
2. readiness probe - обязательно. Сообщает, что сервис готов к приему трафика. Если проба не проходит - pod исключается из балансировки.
3. liveness probe - при необходимости. Сообщает что сервис работает. Использовать если само приложение может перейти в нерабочее состояние. Если проба не проходит - контейнер перезапускается.
4. Обработка SIGTERM - при необходимости. Если есть активные сессии которые нельзя терять.
5. endpoint /metrics - обязательно. Для сбора метрик на базе Prometheus.

## Usefull links:
### Microservices
1. https://12factor.net/
1. https://semver.org/
1. https://keepachangelog.com/en/1.0.0/

### Health-checks
1. https://kubernetes.io/ru/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/
1. https://blog.colinbreck.com/kubernetes-liveness-and-readiness-probes-how-to-avoid-shooting-yourself-in-the-foot/

### Gracefull shutdown
1. https://pracucci.com/graceful-shutdown-of-kubernetes-pods.html
1. https://itnext.io/containers-terminating-with-grace-d19e0ce34290

### Metrics
1. https://prometheus.io/docs/practices/instrumentation/
