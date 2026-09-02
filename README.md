# restful-booker · тестирование REST API

Тестовое задание по публичному учебному API [restful-booker](https://restful-booker.herokuapp.com): тест-кейсы, Postman-коллекция с проверками и баг-репорты по найденным дефектам.

Покрыты все 8 эндпоинтов: **112 кейсов** (27 позитивных, 71 негативный, 14 граничных) и **20 заведённых дефектов**.

## Что внутри

| Файл | Что там |
|---|---|
| [docs/test-cases.md](docs/test-cases.md) | Тест-кейсы по разделам API, сводка покрытия, применённые техники тест-дизайна |
| [docs/test-cases.xlsx](docs/test-cases.xlsx) | Те же кейсы таблицей, для выгрузки в тест-менеджмент |
| [docs/bug-reports.md](docs/bug-reports.md) | 20 баг-репортов со сводкой, шагами воспроизведения и severity |
| [docs/api-spec.md](docs/api-spec.md) | Снапшот документации API, по которому писались ожидаемые результаты |
| [postman/restful-booker.postman_collection.json](postman/restful-booker.postman_collection.json) | Коллекция Postman v2.1 с проверками во вкладке Tests |
| [postman/environment.example.json](postman/environment.example.json) | Образец окружения с переменными стенда |

## Как запустить

1. Импортировать коллекцию в Postman: **Import** → `postman/restful-booker.postman_collection.json`.
2. При необходимости импортировать окружение `postman/environment.example.json` и выбрать его в правом верхнем углу. Без этого коллекция тоже работает: переменные стенда лежат в ней самой.
3. Запустить через **Collection Runner**.

## Прогон намеренно красный

Часть проверок падает, и так задумано. Каждое падение помечено `[BUG-NN]` и ведёт на разбор в [docs/bug-reports.md](docs/bug-reports.md). Зелёный прогон означал бы, что проверки написаны под фактическое поведение стенда, а не под документацию.
