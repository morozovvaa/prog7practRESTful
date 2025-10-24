# Практическая работа - RESTful веб-приложение на Spring Boot

# Цель работы: 
Создать RESTful веб-приложение “Task Management System” для управления задачами с использованием Spring Boot, применяя ключевые компоненты фреймворка, включая автоматическую конфигурацию, стартеры,
работу с базой данных и безопасность.

---

## 1. Структура и Конфигурация

### Используемые компоненты:

* **Язык:** Java 17+
* **Среда:** Spring Boot 3.x
* **База данных:** H2 (in-memory, `jdbc:h2:mem:taskdb`)
* **Слой данных:** Spring Data JPA (`TaskRepository`)
* **Безопасность:** Spring Security (HTTP Basic Authentication)

### Конфигурация безопасности (`SecurityConfig.java`):

Настроена базовая аутентификация, защищающая все конечные точки (`anyRequest().authenticated()`), за исключением H2 Console. Учетные данные (`admin`/`admin123`) определены в `application.yml`.

---

## 2. Проверка Безопасности (Spring Security)

Тестирование конечной точки `GET /api/tasks` доказывает, что настроенная аутентификация работает корректно.

### 1. Доступ без аутентификации

При первой попытке доступа к конечной точке `GET /api/tasks` **без предоставления учетных данных** (логина/пароля) сервер вернул HTTP-статус **401 Unauthorized**. Это подтверждает, что конфигурация Spring Security успешно защищает API от неавторизованного доступа.

**GET /api/tasks (401)**
<img width="974" height="338" alt="image" src="https://github.com/user-attachments/assets/6e7ea69a-57c6-434f-bc21-c21db6fb04ea" />


### 2. Доступ с аутентификацией

После предоставления корректных учетных данных (`admin`/`admin123`) через Basic Authentication, доступ был разрешен, и сервер вернул статус **200 OK** с запрошенными данными (списком задач). Это подтверждает успешную аутентификацию и авторизацию для доступа к ресурсу.

**GET /api/tasks (200 OK)**
<img width="974" height="334" alt="image" src="https://github.com/user-attachments/assets/0af1908b-2dfa-4c61-b92f-e8ccc88e8b3d" />


---

## 3. Демонстрация CRUD-операций (TaskController)

### 3.1. CREATE (Создание Задачи)

* **Метод:** `POST /api/tasks`
* **Результат:** Успешный ответ **200 OK** (или 201 Created) с присвоенным `id` и `createdAt`.

<img width="974" height="510" alt="image" src="https://github.com/user-attachments/assets/73519de8-0a86-4585-8659-8f3b45d4e8f8" />


### 3.2. READ (Чтение Задачи по ID)

* **Метод:** `GET /api/tasks`
* **Результат:** Успешный ответ **200 OK** с полными данными задачи.

<img width="974" height="518" alt="image" src="https://github.com/user-attachments/assets/654c05b2-5231-45fd-a8a3-b329d043b5b1" />


### 3.3. UPDATE (Обновление Задачи)

* **Метод:** `PUT /api/tasks/{id}`
* **Результат:** Успешный ответ **200 OK** с обновленным полем `status` или `description`.

<img width="974" height="491" alt="image" src="https://github.com/user-attachments/assets/1b46571c-95e4-4101-8581-0cf2bc33ff3b" />


### 3.4. DELETE (Удаление Задачи)

* **Метод:** `DELETE /api/tasks/{id}`
* **Результат:** Успешный ответ **200 OK**.

<img width="974" height="368" alt="image" src="https://github.com/user-attachments/assets/852e3a3f-5971-400d-bec2-60cd3e698262" />  

<img width="974" height="395" alt="image" src="https://github.com/user-attachments/assets/72ce96a2-8462-435f-8416-0c054c461f96" />


---

## 4. Проверка Базы Данных (H2 Console)

<img width="974" height="494" alt="image" src="https://github.com/user-attachments/assets/ea6069bf-f841-4848-9d98-a189887106ea" />

<img width="974" height="504" alt="image" src="https://github.com/user-attachments/assets/3b751755-119c-430b-a6c8-4c799cc32e02" />


Доступ к H2 Console был разрешен в `SecurityConfig`, что позволило проверить сохранение данных на уровне базы.

* **URL консоли:** `http://localhost:8080/h2-console`
* **Запрос:** `SELECT * FROM TASK;`

**Результат:** Запрос подтвердил, что данные, отправленные через API (см. п. 3.1), были корректно сохранены в таблице `TASK` и содержат поля `TITLE`, `DESCRIPTION`, `STATUS` и `CREATED_AT`.

<img width="974" height="382" alt="image" src="https://github.com/user-attachments/assets/bc5d31e7-a7c5-4734-b819-b01efefc6ee6" />

