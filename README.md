# Практическая работа - RESTful веб-приложение на Spring Boot

# Цель работы: 
Создать RESTful веб-приложение “Task Management System” для управления задачами с использованием Spring Boot, применяя ключевые компоненты фреймворка, включая автоматическую конфигурацию, стартеры,
работу с базой данных и безопасность.


---

## Использованные команды и их назначение

### 1. Подготовка окружения Ubuntu/WSL

#### Команда:
```bash
sudo apt update
```
**Назначение:** Обновление списков пакетов из репозиториев Ubuntu для получения информации о последних версиях доступных пакетов и их зависимостей.

---

#### Команда:
```bash
sudo apt upgrade -y
```
**Назначение:** Установка обновлений для всех установленных пакетов в системе. Флаг `-y` автоматически подтверждает установку без дополнительных запросов.

---

#### Команда:
```bash
sudo apt install unzip curl git -y
```
**Назначение:** Установка базовых утилит:
- `unzip` - для распаковки архивов
- `curl` - для загрузки файлов из интернета
- `git` - система контроля версий

---

### 2. Установка SDKMAN


---

#### Команда:
```bash
sudo apt install zip unzip -y
```
**Назначение:** Установка недостающих утилит `zip` и `unzip`, необходимых для работы SDKMAN.

---

#### Команда:
```bash
curl -s "https://get.sdkman.io" | bash
```
**Назначение:** Загрузка и запуск скрипта установки SDKMAN - инструмента для управления версиями JDK и Maven.

---

### 3. Инициализация и проверка SDKMAN

#### Команда:
```bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```
**Назначение:** Загрузка скриптов SDKMAN в текущую сессию терминала для активации команды `sdk`.

---

#### Команда:
```bash
sdk version
```
**Назначение:** Проверка успешной установки SDKMAN и отображение установленной версии.

**Результат:**
```
SDKMAN!
script: 5.20.0
native: 0.7.14 (linux x86_64)
```

---

### 4. Установка Java Development Kit (JDK)

#### Команда:
```bash
sdk install java 17.0.8-tem
```
**Назначение:** Установка Java Development Kit версии 17.0.8 от Eclipse Temurin (tem). Java 17 является LTS (Long Term Support) версией, рекомендованной для Spring Boot 3.x.

---

#### Команда:
```bash
sdk default java 17.0.8-tem
```
**Назначение:** Установка Java 17.0.8-tem в качестве версии JDK по умолчанию для всех shell-сессий.

---

### 5. Установка Maven

#### Команда:
```bash
sdk install maven
```
**Назначение:** Установка Apache Maven версии 3.9.11 - инструмента для управления проектами и сборки Java-приложений. Maven автоматически загружает зависимости и компилирует проект.

---

### 6. Проверка установки Java и Maven

#### Команда:
```bash
java -version
```
**Назначение:** Проверка установленной версии Java.

**Результат:**
```
openjdk version "17.0.8" 2023-07-18
OpenJDK Runtime Environment Temurin-17.0.8+7 (build 17.0.8+7)
OpenJDK 64-Bit Server VM Temurin-17.0.8+7 (build 17.0.8+7, mixed mode, sharing)
```

---

#### Команда:
```bash
mvn -version
```
**Назначение:** Проверка установленной версии Maven.

**Результат:**
```
Apache Maven 3.9.11 (3e54c93a704957b63ee3494413a2b544fd3d825b)
Maven home: /home/diamo/.sdkman/candidates/maven/current
Java version: 17.0.8, vendor: Eclipse Adoptium, runtime: /home/diamo/.sdkman/candidates/java/17.0.8-tem
```

---

### 7. Создание структуры проекта

#### Команда:
```bash
mkdir -p ~/projects
```
**Назначение:** Создание директории `projects` в домашней папке пользователя для хранения проектов. Флаг `-p` создает все промежуточные директории, если они не существуют.

---

### 8. Копирование проекта из Windows в WSL

#### Команда:
```bash
ls /mnt/c/Users/diamo/
```
**Назначение:** Просмотр содержимого домашней директории Windows для поиска правильного расположения проекта.

---

#### Команда (успешная):
```bash
cp -r /mnt/c/Users/diamo/projects/demo ~/projects/
```
**Назначение:** Успешное рекурсивное копирование проекта `demo` из Windows в WSL Linux-окружение.

---

### 9. Навигация и просмотр структуры проекта

#### Команда:
```bash
cd ~/projects/demo
```
**Назначение:** Переход в директорию проекта для работы с ним.

---

#### Команда:
```bash
ls -R
```
**Назначение:** Рекурсивный просмотр всей структуры проекта, включая все поддиректории и файлы.

**Результат:** Показана стандартная структура Maven-проекта Spring Boot:
```
.
├── HELP.md
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── com/example/demo
    │   │       └── DemoApplication.java
    │   └── resources
    │       ├── application.properties
    │       ├── static
    │       └── templates
    └── test
        └── java
            └── com/example/demo
                └── DemoApplicationTests.java
```

---

### 10. Запуск Spring Boot приложения

#### Команда:
```bash
./mvnw spring-boot:run
```
**Назначение:** Запуск Spring Boot приложения через Maven Wrapper (`mvnw`). Эта команда выполнила следующие действия:
1. Скачала все необходимые зависимости из Maven Central Repository
2. Скомпилировала исходный код проекта
3. Запустила встроенный Tomcat сервер на порту 8080
4. Инициализировала базу данных H2
5. Запустила приложение Task Management System

**Результат запуска:**
```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/

 :: Spring Boot ::                (v3.5.6)

Started DemoApplication in 2.869 seconds (process running for 3.127)
```

**Детали запуска:**
- Tomcat запущен на порту 8080
- Инициализирована база данных H2 (HikariPool-1)
- Hibernate ORM версия 6.6.29.Final
- JPA EntityManagerFactory инициализирован
- Приложение доступно по адресу `http://localhost:8080`

---

### 11. Открытие проекта в VS Code

#### Команда:
```bash
code .
```
**Назначение:** Открытие текущей директории проекта в Visual Studio Code для просмотра и редактирования исходного кода.

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

