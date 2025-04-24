# Kittygram

## Описание проекта
Kittygram — это социальная сеть для обмена фотографиями котиков. Пользователи могут создавать профили своих питомцев, загружать фото, добавлять достижения и просматривать записи других пользователей. Проект включает REST API на Django, фронтенд на React и NGINX в качестве обратного прокси. Разворачивается с помощью Docker Compose и автоматизированного CI/CD через GitHub Actions.

## Использованные технологии
- **Backend**:
  - Django 4.2.9
  - Django REST Framework 3.14.0
  - PostgreSQL
  - Gunicorn 20.1.0
  - Python-dotenv 1.0.0
  - Pillow 10.2.0
- **Frontend**:
  - React
  - Node.js 18
- **Инфраструктура**:
  - NGINX
  - Docker
  - Docker Compose
  - GitHub Actions (CI/CD)
- **База данных**: PostgreSQL 13
- **Тестирование**: Flake8, Django Test Framework, npm test

## Инструкция по запуску

### Предварительные требования
- Установлены: Docker, Docker Compose, Git
- Доступ к Docker Hub (для загрузки образов)
- Свободные порты: 80 (NGINX), 5432 (PostgreSQL), 8000 (Django)

### Локальный запуск
1. **Клонируйте репозиторий**:
   ```bash
   git clone https://github.com/maksonin65/kittygram_final.git
   cd kittygram_final
   ```
2. **Создайте файл `.env`**:
   Заполните `.env` необходимыми значениями, например:
   ```
   POSTGRES_DB=kittygram
   POSTGRES_USER=kittygram_user
   POSTGRES_PASSWORD=kittygram_password
   DB_NAME=kittygram
   DB_HOST=db
   DB_PORT=5432
   SECRET_KEY=your-secret-key
   DEBUG=False
   ALLOWED_HOSTS=localhost,127.0.0.1
   ```
3. **Запустите Docker Compose**:
   ```bash
   docker-compose up -d
   ```
4. **Примените миграции и соберите статические файлы**:
   ```bash
   docker-compose exec backend python manage.py migrate
   docker-compose exec backend python manage.py collectstatic --noinput
   ```
5. **Откройте приложение**:
   - В браузере перейдите на `http://localhost`.
   - Создайте суперпользователя командой `docker-compose exec backend python manage.py createsuperuser`.
   - Админ-панель доступна по `http://localhost/admin/`.

### Запуск в продакшене
1. Скопируйте `docker-compose.production.yml` и `.env` на сервер.
2. Настройте `.env` с продакшен-значениями (например, `ALLOWED_HOSTS=your-domain.com`).
3. Выполните:
   ```bash
   sudo docker compose -f docker-compose.production.yml pull
   sudo docker compose -f docker-compose.production.yml up -d
   sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
   sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic --noinput
   ```
4. Настройте домен и SSL.

## Примеры запросов к API
API доступно по адресу `/api/`.

### Получение списка питомцев
```bash
curl http://localhost/api/cats/
```
**Ответ** (пример):
```json
[
  {
    "id": 1,
    "name": "Мурзик",
    "color": "Black",
    "birth_year": 2024,
    "owner": "user1",
    "achievements": ["Поймал мышь"],
    "image": "/media/cats/murzik.jpg"
  }
]
```

### Получение токена авторизации
```bash
curl -X POST http://localhost/api/auth/token/login/ \
  -d "username=user1&password=yourpassword"
```
**Ответ** (пример):
```json
{
  "auth_token": "your-token"
}
```

### Создание питомца (требуется авторизация)
```bash
curl -X POST http://localhost/api/cats/ \
  -H "Authorization: Token <your-token>" \
  -F "name=Барсик" \
  -F "color=White" \
  -F "birth_year=2022" \
  -F "image=@/path/to/image.jpg"
```
**Ответ** (пример):
```json
{
  "id": 2,
  "name": "Барсик",
  "color": "White",
  "birth_year": 2023,
  "owner": "user1",
  "achievements": [],
  "image": "/media/cats/barsik.jpg"
}
```


## Информация об авторе
- **Имя**: Максим
- **Контакты**: https://github.com/maksonin65