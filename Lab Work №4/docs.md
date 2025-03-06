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

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

documents = []
users = []
approvals = []

def find_document(doc_id):
    return next((doc for doc in documents if doc["id"] == doc_id), None)

def find_user(user_id):
    return next((user for user in users if user["id"] == user_id), None)

# 1. Получение списка документов (GET /api/v1/documents)
@app.route("/api/v1/documents", methods=["GET"])
def get_documents():
    """ Возвращает список всех документов """
    if not documents:
        return jsonify({"info": "Документы отсутствуют"}), 200
    return jsonify(documents), 200

# 2. Добавление нового документа (POST /api/v1/documents)
@app.route("/api/v1/documents", methods=["POST"])
def create_document():
    """ Добавляет новый документ в систему """
    data = request.json
    if not data.get("title") or not data.get("content") or not data.get("author_id"):
        return jsonify({"error": "Некорректный ввод данных"}), 400

    doc_id = len(documents) + 1
    new_doc = {
        "id": doc_id,
        "title": data["title"],
        "content": data["content"],
        "status": "На согласовании",
        "author_id": data["author_id"]
    }
    documents.append(new_doc)
    return jsonify(new_doc), 201

# 5. Получение списка пользователей (GET /api/v1/users)
@app.route("/api/v1/users", methods=["GET"])
def get_users():
    """ Возвращает список всех пользователей """
    if not users:
        return jsonify({"info": "Пользователи отсутствуют"}), 200
    return jsonify(users), 200

# 6. Добавление нового пользователя (POST /api/v1/users)
@app.route("/api/v1/users", methods=["POST"])
def create_user():
    """ Добавляет нового пользователя """
    data = request.json
    if not data.get("name") or not data.get("role"):
        return jsonify({"error": "Некорректный ввод данных"}), 400

    user_id = len(users) + 1
    new_user = {
        "id": user_id,
        "name": data["name"],
        "role": data["role"]
    }
    users.append(new_user)
    return jsonify(new_user), 201

# 6. Согласование документа (POST /api/v1/approvals)
@app.route("/api/v1/approvals", methods=["POST"])
def approve_document():
    """ Позволяет согласовать документ """
    data = request.json
    doc = find_document(data.get("document_id"))
    user = find_user(data.get("approver_id"))

    if not doc:
        return jsonify({"error": "Документ не найден"}), 404
    if not user:
        return jsonify({"error": "Пользователь не найден"}), 404

    approval = {
        "document_id": doc["id"],
        "approver_id": user["id"],
        "status": data.get("status"),
        "comment": data.get("comment")
    }
    approvals.append(approval)
    doc["status"] = "Согласован" if data.get("status") == "approved" else "Отклонен"

    return jsonify({"message": "Документ согласован", "status": approval["status"]}), 201

if __name__ == "__main__":
    app.run(debug=True)
```

## Тестирование API

Тестирование API проводилось с использование сервиса Postman

### Тест 1: Получение списка документов (когда документов нет)

![image](https://github.com/user-attachments/assets/f545a681-1662-4a65-8530-9101c28f5351)

### Тест 2: Успешное получение списка документов (после добавления документа)

Добавление документа:

![image](https://github.com/user-attachments/assets/a75f874f-fbce-40a5-afad-8e7ccad303c1)

Запрос после добавления документа:

![image](https://github.com/user-attachments/assets/177b8725-73dd-405c-9249-26f849596353)

### Тест 3: Успешное добавление документа

![image](https://github.com/user-attachments/assets/382dd322-e2a5-4a38-a2c1-b78faa1b928e)

### Тест 4: Ошибка при отсутствии обязательных полей

![image](https://github.com/user-attachments/assets/0f9200bd-2f90-4c1d-af75-5c275f7df7c9)

### Тест 5: Получение списка пользователей (когда пользователей нет)

![image](https://github.com/user-attachments/assets/c3073e71-1084-48fa-b43b-004379928733)

### Тест 6: Успешное получение списка пользователей (после добавления пользователя)

Добавление пользователя:

![image](https://github.com/user-attachments/assets/6d03974a-14d7-473b-aa99-f2823148dddc)

Запрос после добавления пользователя:

![image](https://github.com/user-attachments/assets/24450067-0b57-41bb-8265-dd0949fb50be)

### Тест 7: Успешное согласование документа

![image](https://github.com/user-attachments/assets/532f6919-8853-4c8d-8e5d-bed3bc5da6eb)

### Тест 8: Ошибка при согласовании несуществующего документа

![image](https://github.com/user-attachments/assets/a6c5817d-80a3-475b-8082-2e8830b055e6)

### Тест 9: Проверка статуса документа после согласования

![image](https://github.com/user-attachments/assets/de6593ba-7b9a-4202-bf2c-8813e407dc28)

