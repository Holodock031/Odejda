# 🚀 Быстрый запуск Django Backend

## Автоматическая установка (Windows)

### Вариант 1: Полная установка с нуля
```bash
cd backend
setup.bat
```

Этот скрипт автоматически:
- Создаст виртуальное окружение
- Установит все зависимости
- Применит миграции БД
- Создаст админа (admin / admin123)
- Загрузит тестовые товары

### Вариант 2: Быстрый запуск
```bash
cd backend
start.bat
```

Этот скрипт проверит установку и запустит сервер.

---

## Ручная установка

### 1. Создайте виртуальное окружение
```bash
cd backend
python -m venv venv
```

### 2. Активируйте его

Windows:
```bash
venv\Scripts\activate
```

Linux/Mac:
```bash
source venv/bin/activate
```

### 3. Установите зависимости
```bash
pip install -r requirements.txt
```

### 4. Примените миграции
```bash
python manage.py makemigrations
python manage.py migrate
```

### 5. Создайте суперпользователя
```bash
python manage.py createsuperuser
```

Или используйте готового:
```bash
python manage.py shell -c "from django.contrib.auth.models import User; User.objects.create_superuser('admin', 'admin@fashionista.ru', 'admin123')"
```

### 6. Загрузите тестовые данные
```bash
python manage.py load_initial_data
```

### 7. Запустите сервер
```bash
python manage.py runserver
```

---

## 📍 Доступ

- **API**: http://localhost:8000/api/
- **Админка**: http://localhost:8000/admin/
- **Логин**: admin
- **Пароль**: admin123

---

## 🎯 Основные API endpoints

### Товары
- `GET /api/products/` - все товары
- `GET /api/products/1/` - товар по ID
- `GET /api/products/?category=dresses` - фильтр по категории
- `GET /api/products/?search=платье` - поиск

### Категории
- `GET /api/categories/` - все категории

### Заказы (требуется авторизация)
- `GET /api/orders/` - мои заказы
- `POST /api/orders/` - создать заказ

### Авторизация
- `POST /api/auth/register/` - регистрация
- `POST /api/auth/login/` - вход
- `POST /api/auth/logout/` - выход
- `GET /api/auth/user/` - текущий пользователь

---

## 🖼️ Загрузка изображений

В админке можно:
1. Загружать файлы через поле "Изображение"
2. Указывать URL в поле "URL изображения"

Загруженные файлы сохраняются в `media/products/`

---

## 📦 Что включено

✅ Модели: Category, Product, Order, OrderItem, Review, UserProfile
✅ REST API с фильтрацией и поиском
✅ Админ-панель с превью изображений
✅ Авторизация пользователей
✅ CORS для фронтенда
✅ 12 тестовых товаров

---

## 🔧 Структура проекта

```
backend/
├── fashionista/          # Настройки Django
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── shop/                 # Приложение магазина
│   ├── models.py         # Модели БД
│   ├── serializers.py    # DRF сериализаторы
│   ├── views.py          # API views
│   ├── admin.py          # Админка
│   ├── urls.py
│   └── management/
│       └── commands/
│           └── load_initial_data.py
├── media/                # Загруженные файлы
├── manage.py
├── requirements.txt
├── setup.bat            # Автоустановка
└── start.bat            # Быстрый запуск
```

---

## 🎨 Админка

После запуска откройте http://localhost:8000/admin/

Возможности:
- ✅ Управление товарами с загрузкой фото
- ✅ Управление категориями
- ✅ Просмотр и обработка заказов
- ✅ Управление пользователями
- ✅ Модерация отзывов
- ✅ Превью изображений в списках

---

## 🔗 Подключение фронтенда

Обновите JS файлы фронтенда:

```javascript
const API_URL = 'http://localhost:8000/api';

// Получение товаров
fetch(`${API_URL}/products/`)
  .then(res => res.json())
  .then(data => {
    window.products = data.results;
    renderProducts(window.products);
  });

// Создание заказа
fetch(`${API_URL}/orders/`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  credentials: 'include',
  body: JSON.stringify({
    items: cart.map(item => ({
      product_id: item.id,
      quantity: item.quantity
    })),
    customer_name: 'Имя',
    customer_email: 'email@example.com',
    customer_phone: '+7 999 999-99-99',
    delivery_address: 'Адрес доставки'
  })
})
.then(res => res.json())
.then(order => console.log('Заказ создан:', order));
```

---

## ❓ Проблемы?

### Ошибка "python не найден"
Установите Python 3.8+ с https://www.python.org/

### Ошибка при установке Pillow
```bash
pip install --upgrade pip
pip install Pillow
```

### Порт 8000 занят
```bash
python manage.py runserver 8001
```

---

Готово! Backend запущен и готов к работе 🎉
