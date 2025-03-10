# Internship-test-java-Gardash

## Описание проекта
Небольшое REST API для управления балансом пользователей.

## Технологии
- Spring Boot
- Hibernate
- Maven
- PostgreSQL
- Lombok

## Требования
- Java 21
- PostgreSQL 17
- Maven 3.4.1

## Запуск проекта

### Запуск в Docker
Проект можно запустить в Docker, так как были созданы `Dockerfile` и `docker-compose.yml` (для Spring-приложения и PostgreSQL). Используйте команды:

```sh
docker-compose build
```

```sh
docker-compose up
```

или

```sh
docker-compose up --build
```

### Запуск вручную
Также можно запустить приложение вручную с использованием `java -jar`:

```sh
java -jar ./target/InternshipTestJava-0.0.1-SNAPSHOT.jar
```

Но перед этим необходимо изменить `application.properties`, указав корректные настройки подключения к PostgreSQL и пересобрать проект в JAR-файл с помощью Maven:
```sh
mvn clean package 
```

## API

### Эндпоинты для работы с аккаунтами (`/api/accounts`):

- **Создание аккаунта**  
  **POST** `/create`  
  **Request Body:**
  ```json
  {"username":"example_user"}
  ```
  **Response:**
  ```json
  { "id": 1, "username": "example_user", "balance": 0, "createdAt": "2025-03-07T12:00:00.000000", "operationsList": null }
  ```

- **Получение информации об аккаунте**  
  **GET** `/{accountId}/info`  
  **Response:**
  ```json
  {
      "id": 1,
      "username": "example_user",
      "balance": 100.50,
      "createdAt": "2025-03-07T12:00:00.000000",
      "operationsList": [
          {
              "id": 10,
              "amount": 50.00,
              "type": "DEPOSIT",
              "description": "Пополнение счета на сумму: 50",
              "date": "2025-03-07T12:30:00.000000",
              "accountId": 1,
              "accountBalance": 100.50
          }
      ]
  }
  ```

- **Обновление информации об аккаунте**  
  **PUT** `/{accountId}/update`  
  **Request Body:**
  ```json
  {"username":"new_username"}
  ```
  **Response:** аналогично `GET /{accountId}/info`, но с обновленным `username`.

- **Удаление аккаунта**  
  **DELETE** `/{accountId}/delete`  
  **Response:**
  ```json
  {"deleteStatus":"Аккаунт успешно удален!"}
  ```
  При удалении аккаунта также удаляются все связанные операции.

- **Получение текущего баланса**  
  **GET** `/{accountId}/balance`  
  **Response:**
  ```json
  {"currentBalance": 100.50}
  ```

- **Получение баланса на определенную дату**  
  **GET** `/{accountId}/balance-by-date`  
  **Request Param:** `date` в формате `2025-03-07T00:00:00`  
  **Response:**
  ```json
  {"date":"2025-03-07T00:00:00", "accountBalance": 75.25}
  ```

### Эндпоинты для работы с операциями (`/api/operations`):

- **Осуществление операции на аккаунте**  
  **POST** `/{accountId}/make-operation`  
  **Request Params:**
    - `amount`: сумма операции
    - `type`: может быть `DEPOSIT` или `WITHDRAW`

  **Response:**
  ```json
  {
      "id": 4,
      "amount": 600.00,
      "type": "DEPOSIT",
      "description": "Пополнение счета на сумму: 600",
      "date": "2025-03-07T12:45:00.000000",
      "accountId": 3,
      "accountBalance": 600.00
  }
  ```

- **Получение операций аккаунта за определённый период**  
  **GET** `/{accountId}/get-by-period`  
  **Request Params:**
    - `from`: начало периода (формат `2025-03-07T00:00:00`)
    - `to`: конец периода (формат `2025-03-07T00:00:00`)
  
  **Response:**
  ```json
  [
      {
          "id": 1,
          "amount": 54.00,
          "type": "DEPOSIT",
          "description": "Пополнение счета на сумму: 54",
          "date": "2025-03-07T00:28:16.456125",
          "accountId": 1,
          "accountBalance": 54.00
      },
      {
          "id": 2,
          "amount": 1000.00,
          "type": "DEPOSIT",
          "description": "Пополнение счета на сумму: 1000",
          "date": "2025-03-07T01:10:53.519237",
          "accountId": 1,
          "accountBalance": 1054.00
      },
      {
          "id": 3,
          "amount": 600.00,
          "type": "WITHDRAW",
          "description": "Снятие средств с счета на сумму: 600",
          "date": "2025-03-07T01:11:06.511678",
          "accountId": 1,
          "accountBalance": 454.00
      }
  ]
  ```

