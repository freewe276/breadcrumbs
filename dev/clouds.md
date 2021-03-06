# Cloud infrastructure
Этот документ содержит список популярных облачных технологий с очень короткими описаниями их сути


## Docker
Система контейнеризации. Суть в том, чтобы запускать процессы в специальных контейнерах (с помощью системных вызовов cgroupd и namespace), таким образом, процесс в контейнере не имеет произвольного доступа к файловой системе хоста, ему можно задавать ограничения на потребление ресурсов, файловая система такого процесса может отличаться от хостовой и, более того, все необходимые библиотеки теперь можно упаковать в контейнер и сколько угодно процессов с различными требованиями к библиотекам теперь вполне могут уживаться на одном хосте. Также стоит отметить то, что докер позволяет создавать удобные воспроизодимые среды. Больше информации можно почитать на оф.сайте и различных ресурсах.


## Podman
Альтернативная система контейнеризации. Контейнеры совместимы с docker-контейнерами, но образы хранятся в каталоге пользователя. Имеет улучшенную интеграцию с kubernetes.


## docker-compose
Когда у тебя много контейнеров, управление ими превращается в головную боль. Docker-compose позволяет решить эту проблему, описывая взаимоотношения между контейнерами в одном файле конфигурации


## Kubernetes
Мало того, что надо собрать контейнер, его надо еще как-то развернуть в облаке, завставить различные приложения в контейнерах коммуницировать между собой, ... Именно эту задачу и решает Kubernetes. С его помощью можно описывать POD-ы (группы контейнеров, которые будут крутиться вместе), описывать их требования и деплоить в прод. Kubernetes сам будет отвечать за то, чтобы развернуть контейнеры, в случае если кто-то оказался недоступен, на его место будет поднят новый контейнер, что гарантирует отказоустойчиваость кластера


## Terraform
Terraform позволяет описывать инфраструктуру облака в виде файлов конфигурации. С его помощью можно полностью автоматизировать создание инстансов, сетей, баз данных внутри облака. Также он позволяет вносить изменения в облачную инфрастуктуру также на основании конфига. Это, кроме того, позволяет получить версионирование своей инфраструктуры. Имеет привязки к большинству наиболее популярных облачных систем.


## graphite
Система для сбора и построения метрик по time-series-data. Состоит из трех компонентов:  
- *carbon* - сервис, который принимает time-series-данные (альтернатива) и отправляет в whisper. Данные при получении какое-то время хранятся в памяти и балками пишутся на диск  
- *whisper* - БД для хранения  
- *graphite-web* - интерфейс + api, умеет производить графики  
Graphite не занимается сбором данных, только хранением и визуализацией


## statsd + collectd
*Statsd* - приложение для отправки метрик в graphite  
*Collectd* - приложение для отправки метрик в statsd (ставится на каждой машине), умеет собирать метрики от nginx, postgre, apache, from log-files, …  


## grafana - альтернативный веб-интерфейс для графита
Обладает очень удобным интерфейсом для построения графиков, очень хорошее и популярное решение  


## prometheus
Cистема мониторинга и алертинга (замена zabbix). По-умолчанию сервер с прометеем тянет данные от клиентов. Имеется поддержка service-discovery. Имеет огромное количество интеграций.


## zipkin
Распределенная система трэйсинга. Помогает понять как  в микросервисной архитектуре обрабатывался запрос, через какие сервисы проходил, сколько времени заняла обработка на каждом сервисе. На сервисе должен быть развернут Reporter, который будет отправлять данные с UID-ами запросов и временем  в Zipkin. В качестве транспорта можно использовать HTTP, kaffka, …
Изначально использовалась Cassandra, но также можно использовать Mysql, es.
Для исследования трэйсов можно использовать встроенный webui.


## OpenTracing
Открытый стандарт, регламентируюзий формат записи логов.


## ZooKeeper
Распределенное хранилище конфигураций. Конфиги хранятся в виде дерева znode-ов. Каждую такую znode можно обновлять, удалять, создавать, подписываться на обновления, …
Приложения может использовать zookeeper для хранения конфигурации. При старте необходимо обратиться к zookeeper для получения конфигурации. Также можно подписаться на изменение конфигурации и при изменении скачивать новую и перезагружать сервис.


## consul
Key-Value-хранилище с функцией Service-Discovery. Т.е. В отличие от zookeeper, позволяет не только хранить конфигурации, но и динамически обновлять данные конфигурации, при изменении инфраструктуры, переезде между облаками требует меньшее количество усилий для переконфигурирования.
Consul использует gossip в качестве протокола обмена данными. Система распределенная и не имеет единой точки отказа. Для приложения использующего consul необходимо лишь общение с localhost по специальному порту.
Позволяет генерировать конфиг для nginx или Haproxy, а также перегенерирует его при выпадении одной из нод. Таким образом, проверку жизнеспособности ном consul берет на себя


## Ceph
Распределенное хранилище объектов (аналог minio?)


## newRelic, DataDog, Atatus
Альтернативы Kibana и grafana, но более продвинутые. Умеет показывать:  
- графики по сервисам  
- трэйсы запросов и дон информацию по ним  
- можно оценить запросы к БД, которые выполнялись в данный момент времени  
Ссылка на описание возможностей: https://habr.com/ru/company/flant/blog/465177/  
 - это все платные решения, которые разворачиваются не локально, а предоставляются как SaaS  


## Apache Mesos
Link: https://ru.bmstu.wiki/Apache_Mesos
Система децентрализованного выполнения задач. Позволяет планировать распределение ресурсов в кластере и выполнение задач. Поддерживает изоляцию ресурсов через контейнеры, имеется интеграция с ZooKeeper. Состоит из:
 - master node  
 - slave node  
 - scheduler  
 - executor  
(Требует Java)  
В экосистеме mesos существует большое количество фрэймверком для запуска и выполнения задач как долгоживущих, так и задач по расписанию, обработки задач в реальном времени, …   


## service mesh подход
**Проблема:**
В системах с микросервисной архитектурой и большим количеством микросервисов необходимо настроить коммуникацию между микросервисами.
Нужен механизм обнаружения микросервисов, распределения нагрузки (A/B-тестирования, production-staging), механизм повторных запросов,
балансировка трафика в зависимости от нагрузки, исключение падающих сервисов из нагрузки, сбор и аггреграция метрик.
**Решение:**
 - динамическая маршрутизация в зависомости от параметров (production или testing, балансировка нагрузки, выбор ДЦ, ...)  
 - получение списка конечных точек через службу обнаружения  
 - выбор оптимальной точки с использованием правил и метрик  
 - попытка перезапроса на другом экземпляре  
 - убирание сбоящих экземпляров из пула  
 - таймауты по ожиданиям  
 - централизованная система метрик  
 - шифрование, авторизация/аутентификация
**Примеры:**
 - https://istio.io
 - https://linkerd.io
 - https://www.consul.io


## drone.io
CI,CD, предоставляют облако, можно установить в своем облаке. Позволяет описывать процесс сборки приложения в виде кода. Есть пайплайны сборки, хороший набор плагинов, неплохая документация. Выглядит интересно.


## Pulsar
Message-Queue с возможностью стриминга. Kafka хороша производительностью, но через нее нельзя передавать данные в режиме stream-а. Pulsar решает эту проблему. В него можно писать большие файлы, он будет их передавать подписчикам. Имеет встроенную базу данных, поэтому сами инстансы очереди являются stateless.
