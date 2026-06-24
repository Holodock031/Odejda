# 🛍️ Fashionista - Интернет-магазин одежды

Полнофункциональный интернет-магазин с Django backend и современным фронтендом.

## 🚀 Быстрый старт

### 1. Запустите Backend (Django)

```bash
cd backend
setup.bat    # Первый запуск - установка и настройка
start.bat    # Последующие запуски
```

Backend будет доступен на **http://localhost:8000**

### 2. Откройте фронтенд

Просто откройте `index.html` в браузере или используйте Live Server.

### 3. Войдите в админку

- URL: **http://localhost:8000/admin/**
- Логин: **admin**
- Пароль: **admin123**

---

## 📦 Что включено

### Backend (Django)
- ✅ REST API для товаров, категорий, заказов
- ✅ Авторизация и регистрация пользователей
- ✅ Админ-панель с загрузкой изображений
- ✅ База данных SQLite (легко заменить на PostgreSQL)
- ✅ CORS для работы с фронтендом
- ✅ Фильтрация, поиск, сортировка товаров
- ✅ Система отзывов и рейтингов
- ✅ Профили пользователей

### Frontend
- ✅ Современный адаптивный дизайн
- ✅ Каталог товаров с фильтрами
- ✅ Корзина покупок
- ✅ Авторизация и регистрация
- ✅ Личный кабинет
- ✅ Оформление заказов
- ✅ Интеграция с Django API

---

## 🎯 Основные возможности

### Для покупателей
- Просмотр каталога товаров
- Фильтрация по категориям
- Поиск товаров
- Добавление в корзину
- Оформление заказов
- Личный кабинет
- История заказов

### Для администраторов
- Управление товарами
- Загрузка изображений
- Управление категориями
- Обработка заказов
- Управление пользователями
- Модерация отзывов
- Статистика продаж

---

## 📡 API Endpoints

### Товары
```
GET  /api/products/              # Список товаров
GET  /api/products/{id}/         # Детали товара
GET  /api/products/?category=... # Фильтр по категории
GET  /api/products/?search=...   # Поиск товаров
```

### Категории
```
GET  /api/categories/            # Список категорий
GET  /api/categories/{slug}/     # Детали категории
```

### Заказы
```
GET  /api/orders/                # Мои заказы
POST /api/orders/                # Создать заказ
GET  /api/orders/{id}/           # Детали заказа
```

### Авторизация
```
POST /api/auth/register/         # Регистрация
POST /api/auth/login/            # Вход
POST /api/auth/logout/           # Выход
GET  /api/auth/user/             # Текущий пользователь
```

### Профиль
```
GET  /api/profile/               # Получить профиль
PUT  /api/profile/               # Обновить профиль
```

---

## 🔧 Технологии

### Backend
- Python 3.8+
- Django 4.2
- Django REST Framework
- SQLite (можно заменить на PostgreSQL)
- Pillow (для работы с изображениями)

### Frontend
- HTML5, CSS3, JavaScript
- Font Awesome (иконки)
- Адаптивный дизайн

---

## 📁 Структура проекта

```
fashionista/
├── backend/                    # Django backend
│   ├── fashionista/           # Настройки проекта
│   │   ├── settings.py        # Конфигурация
│   │   ├── urls.py            # Главные URL
│   │   └── wsgi.py
│   ├── shop/                  # Приложение магазина
│   │   ├── models.py          # Модели БД
│   │   ├── views.py           # API views
│   │   ├── serializers.py     # DRF сериализаторы
│   │   ├── admin.py           # Админка
│   │   └── urls.py
│   ├── media/                 # Загруженные файлы
│   ├── manage.py
│   ├── requirements.txt
│   ├── setup.bat              # Автоустановка
│   └── start.bat              # Быстрый запуск
│
├── js/
│   ├── api.js                 # API клиент
│   ├── catalog-django.js      # Каталог с API
│   ├── cart.js                # Корзина
│   ├── auth.js                # Авторизация
│   └── main.js                # Основной скрипт
│
├── css/
│   └── style.css              # Стили
│
├── data/
│   └── products.json          # Fallback данные
│
├── index.html                 # Главная страница
├── catalog.html               # Каталог товаров
├── cart.html                  # Корзина
├── auth.html                  # Авторизация
├── profile.html               # Профиль
│
├── README_DJANGO.md           # Этот файл
├── BACKEND_SETUP.md           # Установка backend
└── INTEGRATION_GUIDE.md       # Интеграция фронтенда
```

---

## 🖼️ Работа с изображениями

### Загрузка через админку
1. Откройте http://localhost:8000/admin/
2. Перейдите в "Товары"
3. Выберите товар или создайте новый
4. Загрузите изображение в поле "Изображение"
5. Сохраните

Файлы сохраняются в `backend/media/products/`

### Использование URL
Можно указать внешний URL в поле "URL изображения" (например, с Unsplash).

---

## 🔗 Интеграция фронтенда с API

### Подключение API

Добавьте в HTML перед `</body>`:

```html
<!-- API для работы с Django -->
<script src="js/api.js"></script>
<script src="js/catalog-django.js"></script>
```

### Примеры использования

```javascript
// Получение товаров
fashionistaAPI.getProducts()
    .then(data => console.log(data.results));

// Фильтр по категории
fashionistaAPI.getProducts({ category: 'dresses' })
    .then(data => console.log(data.results));

// Создание заказа
fashionistaAPI.createOrder({
    items: [
        { product_id: 1, quantity: 2 }
    ],
    customer_name: 'Иван Иванов',
    customer_email: 'ivan@example.com',
    customer_phone: '+7 999 999-99-99',
    delivery_address: 'Москва, ул. Примерная, д. 1'
})
.then(order => console.log('Заказ создан:', order));
```

Подробнее см. **INTEGRATION_GUIDE.md**

---

## 🎨 Админ-панель

### Возможности
- ✅ Управление товарами с превью изображений
- ✅ Управление категориями
- ✅ Просмотр и обработка заказов
- ✅ Управление пользователями
- ✅ Модерация отзывов
- ✅ Статистика

### Доступ
- URL: http://localhost:8000/admin/
- Логин: admin
- Пароль: admin123

---

## 💡 Полезные команды

### Backend

```bash
# Активация виртуального окружения
venv\Scripts\activate

# Применить миграции
python manage.py migrate

# Создать суперпользователя
python manage.py createsuperuser

# Загрузить тестовые данные
python manage.py load_initial_data

# Запустить сервер
python manage.py runserver

# Запустить на другом порту
python manage.py runserver 8001
```

---

## 🐛 Отладка

### Проверка API

Откройте консоль браузера (F12):

```javascript
// Проверка подключения
fashionistaAPI.getProducts()
    .then(data => console.log('API работает:', data))
    .catch(error => console.error('Ошибка:', error));
```

### Просмотр запросов
F12 → Network → XHR/Fetch

### Логи Django
Смотрите в терминале, где запущен `python manage.py runserver`

---

## 📚 Документация

- **README.md** - Документация backend
- **BACKEND_SETUP.md** - Детальная установка backend
- **INTEGRATION_GUIDE.md** - Интеграция фронтенда с API
- **backend/QUICK_START.txt** - Краткая справка

---

## 🔒 Безопасность

### Для разработки
- DEBUG = True
- CORS разрешен для всех доменов
- SECRET_KEY в коде

### Для продакшена
Обновите `backend/fashionista/settings.py`:

```python
DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com']
SECRET_KEY = os.environ.get('SECRET_KEY')

CORS_ALLOW_ALL_ORIGINS = False
CORS_ALLOWED_ORIGINS = [
    'https://yourdomain.com',
]
```

---

## 🚀 Деплой

### Backend (Django)
- Heroku
- PythonAnywhere
- DigitalOcean
- AWS

### Frontend
- GitHub Pages
- Netlify
- Vercel

---

## ❓ FAQ

**Q: Как добавить новые товары?**  
A: Через админку: http://localhost:8000/admin/shop/product/add/

**Q: Где хранятся заказы?**  
A: В базе данных `backend/db.sqlite3`, просмотр через админку

**Q: Можно ли использовать PostgreSQL?**  
A: Да, обновите `DATABASES` в `settings.py`

**Q: Как сбросить базу данных?**  
A: Удалите `backend/db.sqlite3` и запустите `setup.bat`

**Q: Порт 8000 занят?**  
A: Используйте `python manage.py runserver 8001`

---

## 📝 Лицензия

MIT License - используйте свободно для своих проектов.

---

## 🎉 Готово!

Backend запущен и готов к работе. Откройте админку и начните добавлять товары!

**Админка:** http://localhost:8000/admin/  
**Логин:** admin  
**Пароль:** admin123

Удачи! 🚀
