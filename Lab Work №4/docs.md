# Лабораторная работа №4

> Тема:  Проектирование REST API
> 
> Цель работы: Получить опыт проектирования программного интерфейса.
> 
> Ожидаемые результаты:
> 
> Для выбранного сервиса системы, предоставляющего API:
> 
> 1. Зафиксировать принятые проектные решения (не менее 8) при проектировании API (оформить в виде документации с четким и подробным описанием в виде документа в md-формате). Использовать методы: GET, POST, PUT, DELETE.
(4 балла)
> 2.Реализовать соответствующий API (c методами GET, POST). Количество методов не менее 6.
(2 балла)
> 3. Протестировать API при помощи Postman (дополнить отчетный документ из п.1 результатами тестирования, сопроводив принтскринами из Postman). Минимум 2 теста на каждый Endpoint.
(2 балла)

# Документация по API

## Описание системы

Сервис предназначен для управления процессом согласования документов в Информационной системе управления землями.
API предоставляет функционал для создания, обновления, получения и удаления документов, а также работы с пользователями и статусами согласования.

## Проектные решения

**1. Архитектурный стиль:** REST API для обеспечения стандартизации и удобства интеграции.

**2. Методы HTTP:** Используются методы GET, POST, PUT, DELETE для реализации CRUD-операций.

**3. Идентификация пользователей:** Авторизация через токены.

**4. Формат данных:** Передача данных в формате JSON.

**5. Обработка ошибок:** Реализована единая структура ошибок с кодами и описанием.

**6. Структура URL:** Четкое разделение по ресурсам, например:

   /documents – управление документами.

   /users – работа с пользователями.

   /approvals – процессы согласования.
   
**8. Статус-коды HTTP:** Использование стандартных кодов для ответа сервера:

   200 OK – успешный запрос.
   
   201 Created – создан новый ресурс.

   400 Bad Request – ошибка в запросе.

   401 Unauthorized – требуется авторизация.

   404 Not Found – ресурс не найден.

   500 Internal Server Error – внутренняя ошибка сервера.
   
**9. Версионирование:** API имеет версию в URL (/api/v1/), чтобы обеспечить обратную совместимость.

**10. Логирование:** Все важные операции фиксируются в системе логирования.

## Эндпоинты API

**1. Получение списка документов**

- *Метод: GET*

- *URL: /api/v1/documents*

- *Описание запроса: Возвращает список всех документов.*

- *Параметры запроса: Отсутствуют.*

**Пример успешного ответа (200):**

```json
[
  {
    "id": 1,
    "title": "Договор аренды",
    "status": "На согласовании",
    "created_at": "2024-02-12T12:00:00Z"
  },
  {
    "id": 2,
    "title": "Акт приемки",
    "status": "Согласовано",
    "created_at": "2024-02-11T15:30:00Z"
  }
]
```

**Ответ при отсутствии документов:**

```json
{
  "info": "Документы отсутствуют"
}
```

**2. Добавление нового документа**

- *Метод: POST*

- *URL: /api/v1/documents*

- *Описание запроса: Добавляет новый документ в систему.*

- *Заголовки: Content-Type: application/json*

**Тело запроса (JSON):**

```json
{
  "title": "Новый договор",
  "content": "Текст договора...",
  "author_id": 3
}
```

**Пример успешного ответа (201):**

```json
{
  "id": 5,
  "title": "Новый договор",
  "status": "На согласовании",
  "created_at": "2024-02-12T16:00:00Z"
}
```

**Ошибка (400 - некорректные данные):**

```json
{
  "error": "Некорректный ввод данных"
}
```

**3. Обновление документа**

- *Метод: PUT*

- *URL: /api/v1/documents/{document_id}*

- *Описание: Обновляет данные документа по его идентификатору.*

- *Заголовки: Content-Type: application/json*

**Тело запроса (JSON):**

```json
{
  "title": "Обновленный договор",
  "content": "Обновленный текст договора..."
}
```

**Пример успешного ответа (200):**

```json
{
  "id": 5,
  "title": "Обновленный договор",
  "status": "На согласовании",
  "updated_at": "2024-02-12T16:30:00Z"
}
```

**Ошибка (404 - документ не найден):**

```json
{
  "error": "Документ не найден"
}
```

**4. Удаление документа**

- *Метод: DELETE*

- *URL: /api/v1/documents/{document_id}*

- *Описание: Удаляет документ из системы.*

- *Параметры запроса: Отсутствуют.*

**Пример успешного ответа (200):**

```json
{
  "message": "Документ успешно удален"
}
```

**Ошибка (404 - документ не найден):**

```json
{
  "error": "Документ не найден"
}
```

**5. Получение списка пользователей**

- *Метод: GET*

- *URL: /api/v1/users*

- *Описание: Возвращает список пользователей, участвующих в согласовании.*

- *Параметры запроса: Отсутствуют.*

**Пример успешного ответа (200):**

```json
[
  {
    "id": 1,
    "name": "Иван Петров",
    "role": "Юрист"
  },
  {
    "id": 2,
    "name": "Марина Смирнова",
    "role": "Координатор"
  }
]
```

**Пример ошибки (500 - внутренняя ошибка сервера):**

```json
{
  "error": "Internal Server Error"
}
```

**Пример ошибки (403 - недостаточно прав):**

```json
{
  "error": "Access denied"
}
```

**Пример ошибки (401 - неавторизованный доступ):**

```json
{
  "error": "Unauthorized. Please provide a valid token."
}
```

**6. Согласование документа**

- *Метод: POST*

- *URL: /api/v1/approvals*

- *Описание: Позволяет согласовать документ.*

- *Заголовки: Content-Type: application/json*

**Тело запроса (JSON):**

```json
{
  "document_id": 5,
  "approver_id": 2,
  "status": "approved",
  "comment": "Документ согласован без замечаний"
}
```

**Пример успешного ответа (201):**

```json
{
  "message": "Документ согласован",
  "status": "approved"
}
```

**Ошибка (404 - документ не найден):**

```json
{
  "error": "Документ не найден"
}
```

## Реализация API

В рамках лабораторной работы представим упрощенную структуру проекта

**Файл Models/Document.cs:**

Модель документа:

```csharp
using System;

namespace DocumentApprovalAPI.Models
{
    public class Document
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        public string Status { get; set; } = "На согласовании";
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
    }
}
```

**Файл Data/AppDbContext.cs:**

Создание контекста базы данных

```csharp
using Microsoft.EntityFrameworkCore;
using DocumentApprovalAPI.Models;

namespace DocumentApprovalAPI.Data
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

        public DbSet<Document> Documents { get; set; }
    }
}
```

**Файл Controllers/DocumentsController.cs:**

Настройка контроллера документов
   
```csharp
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using DocumentApprovalAPI.Data;
using DocumentApprovalAPI.Models;
using Microsoft.EntityFrameworkCore;

namespace DocumentApprovalAPI.Controllers
{
    [Route("api/v1/documents")]
    [ApiController]
    public class DocumentsController : ControllerBase
    {
        private readonly AppDbContext _context;

        public DocumentsController(AppDbContext context)
        {
            _context = context;
        }

        // 1. Получение списка документов (GET /api/v1/documents)
        [HttpGet]
        public async Task<ActionResult<IEnumerable<Document>>> GetDocuments()
        {
            var documents = await _context.Documents.ToListAsync();
            if (documents.Count == 0)
            {
                return Ok(new { info = "Документы отсутствуют" });
            }
            return Ok(documents);
        }

        // 2. Получение документа по ID (GET /api/v1/documents/{id})
        [HttpGet("{id}")]
        public async Task<ActionResult<Document>> GetDocument(int id)
        {
            var document = await _context.Documents.FindAsync(id);
            if (document == null)
            {
                return NotFound(new { error = "Документ не найден" });
            }
            return Ok(document);
        }

        // 3. Добавление нового документа (POST /api/v1/documents)
        [HttpPost]
        public async Task<ActionResult<Document>> CreateDocument([FromBody] Document document)
        {
            if (string.IsNullOrEmpty(document.Title) || string.IsNullOrEmpty(document.Content))
            {
                return BadRequest(new { error = "Некорректный ввод данных" });
            }

            _context.Documents.Add(document);
            await _context.SaveChangesAsync();

            return CreatedAtAction(nameof(GetDocument), new { id = document.Id }, document);
        }

        // 4. Обновление документа (PUT /api/v1/documents/{id})
        [HttpPut("{id}")]
        public async Task<IActionResult> UpdateDocument(int id, [FromBody] Document updatedDocument)
        {
            var document = await _context.Documents.FindAsync(id);
            if (document == null)
            {
                return NotFound(new { error = "Документ не найден" });
            }

            document.Title = updatedDocument.Title;
            document.Content = updatedDocument.Content;
            document.Status = updatedDocument.Status;
            
            await _context.SaveChangesAsync();

            return Ok(document);
        }

        // 5. Удаление документа (DELETE /api/v1/documents/{id})
        [HttpDelete("{id}")]
        public async Task<IActionResult> DeleteDocument(int id)
        {
            var document = await _context.Documents.FindAsync(id);
            if (document == null)
            {
                return NotFound(new { error = "Документ не найден" });
            }

            _context.Documents.Remove(document);
            await _context.SaveChangesAsync();

            return Ok(new { message = "Документ успешно удален" });
        }
    }
}
```

**Файл Program.cs:**

Настройка базы данных

```csharp
using Microsoft.EntityFrameworkCore;
using DocumentApprovalAPI.Data;

var builder = WebApplication.CreateBuilder(args);

// Подключаем базу данных SQLite
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlite("Data Source=documents.db"));

builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

app.UseSwagger();
app.UseSwaggerUI();

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

// Создаем БД при запуске
using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
    db.Database.Migrate();
}

app.Run();
```

## Тестирование API

Тестирование API проводилось с использование сервиса Postman:

<
По каждому реализуемому API предоставить следующую информацию: 
Тестируемое API.
Метод.
Строка запроса, используемая для тестирования. Можно представить в виде текстовой строки или принтскрина из Postman, чтобы продемонстрировать все передаваемые данные.
Принтскрин из Postman передаваемых заголовков и параметров (Params, Authorization, Headers, Body).
Принтскрины из Postman полученного ответа (Body и Headers).
Код автотестов (на получение возвращаемого статуса и содержимого ответа).
Принтскрины из Postman результатов тестирования (Test Results).
>






