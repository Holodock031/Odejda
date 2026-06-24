# 🔗 Руководство по интеграции фронтенда с Django Backend

## Быстрый старт

### 1. Запустите Django backend

```bash
cd backend
setup.bat    # Первый запуск
# или
start.bat    # Последующие запуски
```

Backend будет доступен на http://localhost:8000

### 2. Подключите API к фронтенду

Добавьте в ваши HTML файлы (index.html, catalog.html и т.д.) перед закрывающим тегом `</body>`:

```html
<!-- API для работы с Django -->
<script src="js/api.js"></script>
<script src="js/catalog-django.js"></script>
```

Замените или закомментируйте старый `catalog.js`:

```html
<!-- <script src="js/catalog.js"></script> -->
<script src="js/catalog-django.js"></script>
```

### 3. Готово!

Теперь ваш фронтенд работает с Django API.

---

## Что изменилось

### Товары загружаются из Django
- Вместо `data/products.json` данные приходят из `/api/products/`
- Изображения можно загружать через админку
- Фильтрация и поиск работают на стороне сервера

### Авторизация через Django
- Регистрация и вход через API
- Сессии хранятся на сервере
- Безопасная аутентификация

### Заказы сохраняются в БД
- Все заказы в базе данных
- Просмотр в админке
- История заказов пользователя

---

## API Endpoints

### Товары
```javascript
// Все товары
fashionistaAPI.getProducts()

// Фильтр по категории
fashionistaAPI.getProducts({ category: 'dresses' })

// Поиск
fashionistaAPI.getProducts({ search: 'платье' })

// Сортировка
fashionistaAPI.getProducts({ ordering: '-price' })

// Товар по ID
fashionistaAPI.getProduct(1)
```

### Категории
```javascript
// Все категории
fashionistaAPI.getCategories()
```

### Авторизация
```javascript
// Регистрация
fashionistaAPI.register({
    username: 'user',
    email: 'user@example.com',
    password: 'password123',
    password_confirm: 'password123',
    first_name: 'Имя',
    phone: '+7 999 999-99-99'
})

// Вход
fashionistaAPI.login('username', 'password')

// Выход
fashionistaAPI.logout()

// Текущий пользователь
fashionistaAPI.getCurrentUser()
```

### Заказы
```javascript
// Создание заказа
fashionistaAPI.createOrder({
    items: [
        { product_id: 1, quantity: 2, size: 'M', color: 'Черный' },
        { product_id: 3, quantity: 1 }
    ],
    customer_name: 'Иван Иванов',
    customer_email: 'ivan@example.com',
    customer_phone: '+7 999 999-99-99',
    delivery_address: 'Москва, ул. Примерная, д. 1, кв. 10'
})

// Мои заказы
fashionistaAPI.getOrders()
```

### Профиль
```javascript
// Получить профиль
fashionistaAPI.getProfile()

// Обновить профиль
fashionistaAPI.updateProfile({
    phone: '+7 999 999-99-99',
    address: 'Новый адрес'
})
```

---

## Обновление существующего кода

### Пример: Оформление заказа

Старый код (localStorage):
```javascript
function checkout() {
    const order = {
        id: Date.now(),
        items: [...window.cart],
        total: calculateTotal()
    };
    
    localStorage.setItem('order', JSON.stringify(order));
    alert('Заказ оформлен!');
}
```

Новый код (Django API):
```javascript
async function checkout() {
    try {
        const order = await fashionistaAPI.createOrder({
            items: window.cart.map(item => ({
                product_id: item.id,
                quantity: item.quantity
            })),
            customer_name: document.getElementById('name').value,
            customer_email: document.getElementById('email').value,
            customer_phone: document.getElementById('phone').value,
            delivery_address: document.getElementById('address').value
        });
        
        // Очищаем корзину
        window.cart = [];
        saveCart();
        
        alert(`Заказ №${order.id} успешно оформлен!`);
        
    } catch (error) {
        alert('Ошибка при оформлении заказа: ' + error.message);
    }
}
```

### Пример: Авторизация

Старый код:
```javascript
function login() {
    const email = document.getElementById('email').value;
    const password = document.getElementById('password').value;
    
    // Проверка в localStorage
    const users = JSON.parse(localStorage.getItem('users') || '[]');
    const user = users.find(u => u.email === email && u.password === password);
    
    if (user) {
        localStorage.setItem('currentUser', JSON.stringify(user));
        alert('Вход выполнен!');
    }
}
```

Новый код:
```javascript
async function login() {
    const username = document.getElementById('username').value;
    const password = document.getElementById('password').value;
    
    try {
        const response = await fashionistaAPI.login(username, password);
        alert(`Добро пожаловать, ${response.user.username}!`);
        
        // Перенаправление на главную
        window.location.href = 'index.html';
        
    } catch (error) {
        alert('Ошибка входа: ' + error.message);
    }
}
```

---

## Работа с изображениями

### Загрузка через админку

1. Откройте http://localhost:8000/admin/
2. Войдите (admin / admin123)
3. Перейдите в "Товары"
4. Выберите товар
5. Загрузите изображение в поле "Изображение"
6. Сохраните

Изображение будет доступно по URL: `http://localhost:8000/media/products/filename.jpg`

### Использование URL изображений

Можно также указать внешний URL в поле "URL изображения" (например, с Unsplash).

---

## Отладка

### Проверка подключения к API

Откройте консоль браузера (F12) и выполните:

```javascript
// Проверка доступности API
fashionistaAPI.getProducts()
    .then(data => console.log('API работает:', data))
    .catch(error => console.error('API недоступен:', error));
```

### Проверка авторизации

```javascript
fashionistaAPI.getCurrentUser()
    .then(user => console.log('Текущий пользователь:', user))
    .catch(error => console.log('Не авторизован'));
```

### Просмотр запросов

В консоли браузера (F12) → вкладка Network → фильтр XHR/Fetch

---

## CORS и безопасность

### Разработка
В режиме разработки CORS разрешен для всех доменов.

### Продакшн
Для продакшна обновите `backend/fashionista/settings.py`:

```python
CORS_ALLOW_ALL_ORIGINS = False
CORS_ALLOWED_ORIGINS = [
    'https://yourdomain.com',
]
```

---

## Fallback режим

Если Django backend недоступен, фронтенд автоматически переключится на локальные данные из `data/products.json`.

Это позволяет работать с сайтом даже без запущенного backend.

---

## Структура файлов

```
fashionista/
├── backend/                  # Django backend
│   ├── fashionista/         # Настройки
│   ├── shop/                # Приложение магазина
│   ├── media/               # Загруженные файлы
│   ├── manage.py
│   ├── setup.bat            # Установка
│   └── start.bat            # Запуск
│
├── js/
│   ├── api.js              # API клиент (НОВЫЙ)
│   ├── catalog-django.js   # Каталог с API (НОВЫЙ)
│   ├── catalog.js          # Старый каталог (можно удалить)
│   ├── cart.js
│   ├── auth.js
│   └── main.js
│
├── index.html
├── catalog.html
├── cart.html
└── auth.html
```

---

## Часто задаваемые вопросы

### Q: Нужно ли удалять старые JS файлы?
A: Нет, можно оставить для fallback режима. Просто подключите новые файлы.

### Q: Как добавить новые товары?
A: Через админку Django: http://localhost:8000/admin/shop/product/add/

### Q: Где хранятся заказы?
A: В базе данных SQLite: `backend/db.sqlite3`. Просмотр через админку.

### Q: Можно ли использовать PostgreSQL?
A: Да, обновите `DATABASES` в `settings.py`.

### Q: Как сбросить базу данных?
A: Удалите `backend/db.sqlite3` и запустите `setup.bat` заново.

---

## Поддержка

Если возникли проблемы:
1. Проверьте, запущен ли Django сервер
2. Откройте консоль браузера (F12) для просмотра ошибок
3. Проверьте логи Django в терминале
4. Убедитесь, что порт 8000 не занят

---

Готово! Ваш сайт теперь работает с полноценным Django backend 🎉
