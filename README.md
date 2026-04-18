# Лабораторная работа №29. REST API: контроллеры и маршруты
## Основная информация
**ФИО:** Cтарцева П.С.

**Группа:** ИСП-231

**Дата:** 18.04.2026

## В ходе выполнения лабораторной работы изучили:
- Принципы архитектуры REST и требования к RESTful API
- Создание контроллеров с полным набором CRUD-маршрутов
- Различия между Minimal API и Controller-based API
- Тестирование API через Swagger UI и расширение REST Client
- Правильное использование HTTP-статусов и форматов ответов
- Работа с DTO (Data Transfer Object) для защиты API
- Настройка CORS для взаимодействия с фронтендом

## Структура проекта
```
Lab29_RestAPI/
├── TaskApi/
│   ├── Controllers/
│   │   └── TasksController.cs
│   ├── Models/
│   │   ├── TaskItem.cs
│   │   ├── CreateTaskDto.cs
│   │   └── UpdateTaskDto.cs
│   ├── Properties/
│   │   └── launchSettings.json
│   ├── Program.cs
│   ├── appsettings.Development.json
│   ├── appsettings.json
│   ├── TaskApi.http
│   ├── requests.http
│   └── TaskApi.csproj
├── img/
├── .gitignore
├── .editorconfig
└── README.md
```

## Список всех реализованных маршрутов
| Метод | URL | Действие | HTTP статус |
|---|---|---|---|
| GET | /api/tasks | Получить все задачи | 200 OK |
| GET | /api/tasks?completed=true/false | Получить задачи с фильтрацией по статусу | 200 OK |
| GET | /api/tasks/{id} | Получить задачу по ID | 200 OK / 404 Not Found |
| GET | /api/tasks/search?query={text} | Поиск задач по тексту | 200 OK / 400 Bad Request |
| GET | /api/tasks/priority/{level} | Получить задачи по приоритету | 200 OK / 400 Bad Request |
| GET | /api/tasks/stats | Получить статистику по задачам | 200 OK |
| GET | /api/tasks/sorted?by=priority&desc=false | Получить отсортированные задачи | 200 OK |
| POST | /api/tasks | Создать новую задачу | 201 Created / 400 Bad Request |
| PUT | /api/tasks/{id} | Полностью обновить задачу | 200 OK / 400 Bad Request / 404 Not Found |
| PATCH | /api/tasks/{id}/complete | Переключить статус выполнения | 200 OK / 404 Not Found |
| DELETE | /api/tasks/{id} | Удалить задачу | 204 No Content / 404 Not Found |

## Итоговая таблица `ASP.NET` Core Controller-based API
|Аспект|`ASP.NET` Core Controllers|
|--------|--------------------------|
| Маршруты | `[HttpGet]` атрибут над методом |
| Группировка маршрутов | Класс-контроллер |
| Базовый URL | `[Route("api/[controller]")]` |
| Параметр пути | `(int id)` — параметр метода |
| Параметр запроса | `[FromQuery] bool? completed` |
| Тело запроса | `[FromBody] CreateTaskDto dto` |
| Ответ 200 | `return Ok(data)` |
| Ответ 201 | `return CreatedAtAction(...)` |
| Ответ 404 | `return NotFound(...)` |
| Ответ 204 | `return NoContent()` |
| Типизация данных | Строгая (C#) |
| Документация | Swagger — устанавливается отдельно |

## Главные выводы
1. **REST — не протокол, а архитектурный стиль.** Те же принципы работают с любым языком и фреймворком. Это значит, что понимание REST позволяет создавать API на любой технологии — будь то C#, JavaScript, Python или Go.
2. **Контроллер в `ASP.NET` Core = Router в Express, только с автоматической документацией и строгой типизацией.** В отличие от JavaScript, где типы проверяются только во время выполнения, C# позволяет отловить ошибки ещё на этапе компиляции, что повышает надёжность приложения.
3. **DTO защищает API от некорректных данных:** клиент передаёт только то, что сервер разрешает принять. Это важный принцип безопасности — сервер никогда не должен доверять клиенту и принимать любые поля, которые тот отправляет.
4. **Swagger UI позволяет тестировать API без Postman и без написания тестового JavaScript-кода.** Это значительно ускоряет разработку и отладку, так как документация всегда актуальна и соответствует коду.
5. **Правильные HTTP-статусы — часть «контракта» API.** Клиент должен понимать, что произошло, по коду ответа. Возврат всегда 200 OK сбивает с толку и усложняет обработку ошибок на фронтенде.