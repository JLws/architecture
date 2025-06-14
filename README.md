### **1. Які основні сутності ви б створили?**

- **User (Користувач)**: Бренди, креатори, адміністратори (`id`, `email`, `role`, `company_name` для брендів, `portfolio_url` для креаторів).
- **Campaign (Кампанія)**: Кампанії брендів (`id`, `brand_id`, `title`, `budget`, `status`).
- **Application (Заявка)**: Заявки креаторів (`id`, `campaign_id`, `creator_id`, `status`, `proposal`).
- **Portfolio (Портфоліо)**: Роботи креаторів (`id`, `creator_id`, `media_urls`).
- **Message (Повідомлення)**: Для месенджера (`id`, `sender_id`, `receiver_id`, `content`).

---

### **2. Як реалізували б авторизацію та розмежування доступів?**

- **Авторизація**: JWT-токени
- **Розмежування доступу (RBAC)**:
  - Ролі: `brand`, `creator`, `admin`.
  - Бренди: керують своїми кампаніями, переглядають заявки/аналітику.
  - Креатори: подають заявки, керують портфоліо.
  - Адміни: повний доступ, модерація.

---

### **3. Як би спроєктували систему з урахуванням майбутніх фіч?**

- **Модульність**: Мікросервіси для кожної функції (кампанії, месенджер, аналітика).
- **Месенджер**:
  - Окремий мікросервіс із WebSocket (Socket.IO) і MongoDB для повідомлень.
- **Аналітика**:
  - Мікросервіс із ClickHouse для швидких запитів.
  - Обробка подій через Kafka (наприклад, `post.clicked`).
- **Заявки**:
  - Вбудовано в Application Service.
- **Гнучкість**: event-driven підхід (RabbitMQ), API Gateway для розширення.

---

### **4. Який стек і підходи обрали б і чому?**

- **Мова**: Node.js (TypeScript).
- **Бази даних**:
  - PostgreSQL: основні дані.
  - MongoDB: повідомлення (високий write throughput).
  - ClickHouse: аналітика (швидкі запити).
  - Redis: кеш, сесії.
- **Черга**: RabbitMQ для асинхронних подій.
- **API**: REST.
- **Інфраструктура**: Kubernetes (масштабування), Prometheus/Grafana (моніторинг).
- **Підходи**: Мікросервіси.
- **Чому**: Забезпечує швидку розробку, масштабування, підтримку нових фіч.

---

### **5. Як би зробили систему стабільною та придатною до масштабування?**

- **Стабільність**:
  - **Моніторинг**: Prometheus/Grafana для метрик.
  - **Логування**: ClickHouse та Grafana.
  - **Тестування**: Unit- та інтеграційні тести, нагрузочне тестування.
- **Масштабування**:
  - **Горизонтальне**: Kubernetes.
  - **Кешування**: Redis для частих запитів.
  - **Rate Limiting**: Обмеження запитів через API Gateway.
