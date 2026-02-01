# Blob-LogAnalysis
Описание проекта:
Проект предназначен для сбора, анализа и визуализации логов OpenStack Nova с использованием стека ELK (Elasticsearch, Logstash, Kibana). Система позволяет мониторить производительность API, отслеживать ошибки и анализировать активность в облачной инфраструктуре.

Основные возможности:
Сбор логов: Автоматический сбор логов Nova API и Nova Compute
Парсинг данных: Извлечение структурированных полей из неформатированных логов
Хранение: Централизованное хранение в Elasticsearch
Визуализация: Интерактивные дашборды в Kibana
Анализ: Статистика по API запросам, ошибкам, производительности

Структура проекта:
opensearch-lab/
├── docker-compose.yml # Конфигурация Docker контейнеров
├── logstash.conf # Конфигурация Logstash для парсинга логов
├── datasets/ # Исходные файлы логов
│   └── nova.log # Логи OpenStack Nova
├── config/ # Дополнительные конфигурации
└── README.md # Документация проекта

Пример данных:
{
  "log_file": "nova-api.log.1.2017-05-16_13:53:08",
  "@timestamp": "2017-05-16T00:00:00.008Z",
  "pid": 25746,
  "log_level": "INFO",
  "module": "nova.osapi_compute.wsgi.server",
  "client_ip": "10.11.10.1",
  "http_method": "GET",
  "url": "/v2/54fadb412c4e40cdbaed9335e4c35a9e/servers/detail",
  "response_code": 200,
  "response_time": 0.2477829
}

Управление контейнерами:
# Запуск всех сервисов
docker-compose up -d
# Остановка всех сервисов
docker-compose down
# Просмотр логов
docker-compose logs -f
docker-compose logs elasticsearch --tail=20
docker-compose logs logstash --tail=20
# Перезапуск конкретного сервиса
docker-compose restart logstash

Работа с Elasticsearch:
# Проверка здоровья кластера
curl http://localhost:9200/_cluster/health?pretty
# Список индексов
curl "http://localhost:9200/_cat/indices?v"
# Поиск данных
curl "http://localhost:9200/nova-logs-*/_search?size=5&pretty"
# Статистика по полям
curl -s "http://localhost:9200/nova-logs-*/_search?size=0" -H 'Content-Type: application/json' -d'
{
  "aggs": {
    "response_codes": {
      "terms": {
        "field": "response_code",
        "size": 10
      }
    }
  }
}'

Работа с Kibana:
# Проверка доступности Kibana
curl -I http://localhost:5601
# Открыть Kibana в браузере
# URL: http://localhost:5601
