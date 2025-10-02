Сервис сквозной аналитики для E-commerce «AnalyticaFlow»
Описание проекта: Проект представляет собой программное средство для построения сквозной аналитики в сфере электронной коммерции. Основой проекта является веб-приложение, которое интегрирует данные из различных рекламных каналов, CRM-систем и e-commerce платформ, предоставляя единый интерфейс для анализа эффективности маркетинга и отслеживания полного пути клиента.

Цель программного средства: Повысить рентабельность маркетинговых инвестиций (ROMI) для e-commerce проектов на 25% за счет автоматизации сбора данных, глубокого анализа воронки продаж и оптимизации рекламных кампаний. Сократить время на подготовку сводной отчетности с нескольких часов до 3-5 минут, предоставляя бизнесу актуальные данные для принятия решений в реальном времени.

Основные возможности: Программное средство предоставляет следующий функционал:

Автоматический сбор данных из рекламных систем (Google Ads, Facebook Ads и др.), веб-аналитики (Google Analytics) и e-commerce платформ (Shopify, WooCommerce и др.).

Построение единого отчета по ключевым метрикам: LTV, CAC, ROMI, CPA.

Визуализация воронки продаж и пути клиента (Customer Journey Map).

Мультиканальная атрибуция для оценки вклада каждого рекламного канала.

Настраиваемые дашборды с гибкой системой виджетов.

Когортный анализ для отслеживания удержания клиентов.

Интеграция с CRM для обогащения данных оффлайн-продажами.

Ссылки на репозитории сервера и клиента

Серверная часть: https://github.com/YourUsername/AnalyticaFlow_Server

Клиентская часть: https://github.com/YourUsername/AnalyticaFlow_Client

Содержание
Архитектура

C4-модель

Схема данных

Функциональные возможности

Диаграмма вариантов использования

User-flow диаграммы

Детали реализации

UML-диаграммы

Спецификация API

Безопасность

Оценка качества кода

Тестирование

Unit-тесты

Интеграционные тесты

Установка и запуск

Манифесты для сборки docker образов

Манифесты для развертывания k8s кластера

Лицензия

Контакты

Архитектура
C4-модель
Контейнерный уровень архитектуры ПС

Сервис сквозной аналитики для E-commerce «AnalyticaFlow» представляет собой многокомпонентную информационную систему, обеспечивающую взаимодействие между тремя основными группами пользователей: маркетологами, руководителями бизнеса и администраторами системы.

Маркетологи являются основными пользователями системы. Они подключают источники данных, настраивают отчеты и дашборды, анализируют эффективность рекламных кампаний, проводят когортный анализ и оценивают модели атрибуции для оптимизации маркетингового бюджета.

Руководители используют систему для мониторинга ключевых бизнес-показателей (KPI) на высокоуровневых дашбордах. Это позволяет им принимать стратегические решения на основе агрегированных данных о выручке, затратах и рентабельности.

Администраторы отвечают за управление учетными записями пользователей, мониторинг состояния системы, управление глобальными настройками и обеспечение технической поддержки.

Архитектура системы включает следующие компоненты:

Клиентское приложение AnalyticaFlow_Client (JavaScript, React) — это Single Page Application (SPA), предоставляющее интерактивный интерфейс для визуализации данных, создания кастомных отчетов, настройки дашбордов и управления интеграциями.

Серверная часть AnalyticaFlow_Server (Python, Django Rest Framework) — содержит основную бизнес-логику, включая обработку API-запросов, управление пользователями, и оркестрацию процессов сбора и обработки данных.

Система хранения данных состоит из:

PostgreSQL: реляционная база данных для хранения метаданных: информация о пользователях, проектах, настройках дашбордов и учетных данных для интеграций.

ClickHouse: колоночная аналитическая база данных, предназначенная для хранения больших объемов сырых и агрегированных данных о событиях (клики, показы, сессии, продажи) и их быстрой обработки.

Внешние сервисы, с которыми система интегрируется для получения данных:

Рекламные платформы (Google Ads, Facebook Ads): для получения данных о расходах, кампаниях, показах и кликах.

Системы веб-аналитики (Google Analytics): для сбора данных о поведении пользователей на сайте.

E-commerce платформы (Shopify, WooCommerce): для получения данных о заказах, товарах и выручке.

CRM-системы (AmoCRM, Bitrix24): для обогащения данных информацией о продажах и статусах сделок.

Компонентный уровень архитектуры ПС

На уровне компонентов серверная часть реализована в виде микросервисной архитектуры для обеспечения масштабируемости и отказоустойчивости.

API Gateway (Nginx): распределяет входящие запросы от клиентских приложений к соответствующим микросервисам.

Микросервисы (DRF Services):

Сервис авторизации: управляет аутентификацией (JWT) и авторизацией (RBAC). Хранит данные пользователей в PostgreSQL и сессионные токены в Redis.

Сервис интеграций (Data Collector): отвечает за подключение к внешним API, получение и первичную валидацию данных. Использует RabbitMQ для постановки задач на сбор данных в очередь.

Сервис обработки данных (ETL): извлекает сырые данные из очереди RabbitMQ, очищает, трансформирует, обогащает их и загружает в аналитическую БД ClickHouse.

Сервис аналитики: выполняет сложные аналитические запросы к ClickHouse для расчета метрик (ROMI, LTV, CAC), построения воронок и моделей атрибуции. Предоставляет данные для API отчетов.

Сервис отчетов и дашбордов: предоставляет REST API для клиентского приложения, управляет сохранением и загрузкой конфигураций дашбордов из PostgreSQL.

Сервис уведомлений: отправляет пользователям email-оповещения (например, о падении KPI или ошибках интеграции) через внешний сервис (SendGrid) и брокер сообщений RabbitMQ.

Уровень хранения данных:

PostgreSQL: основная БД для хранения структурированных данных (пользователи, проекты, настройки).

ClickHouse: аналитическая СУБД для хранения event stream'ов и выполнения быстрых OLAP-запросов.

Redis: хранилище типа "ключ-значение" для кэширования, хранения сессий и управления очередями.

Инфраструктурные компоненты:

RabbitMQ: брокер сообщений для организации асинхронного взаимодействия между сервисами (сбор данных, обработка, отправка уведомлений).

Схема данных
Система использует две основные базы данных: PostgreSQL для операционных данных и ClickHouse для аналитических.

PostgreSQL Schema (основные сущности):

users (id, email, password_hash, role)

projects (id, name, owner_id)

project_members (user_id, project_id, permission_level)

integrations (id, project_id, type, settings_json, encrypted_credentials, status)

dashboards (id, project_id, name, config_json)

ClickHouse Schema (основная таблица событий):

Таблица events денормализована для скорости аналитических запросов.

event_date (Date) - Дата события

event_datetime (DateTime) - Дата и время события

project_id (UInt64) - ID проекта

source_type (String) - Тип источника (e.g., 'google_ads', 'facebook', 'shopify')

event_type (String) - Тип события (e.g., 'impression', 'click', 'session_start', 'add_to_cart', 'purchase')

campaign_name (String) - Название кампании

ad_creative_name (String) - Название объявления

cost (Decimal) - Затраты

revenue (Decimal) - Выручка

transactions (UInt32) - Количество транзакций

client_id (String) - Идентификатор клиента

attributes (Map(String, String)) - Дополнительные атрибуты в формате ключ-значение

Пример SQL-скрипта для генерации таблиц в PostgreSQL:

SQL

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role VARCHAR(50) NOT NULL DEFAULT 'marketer',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE projects (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    owner_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE integrations (
    id SERIAL PRIMARY KEY,
    project_id INTEGER REFERENCES projects(id) ON DELETE CASCADE,
    type VARCHAR(100) NOT NULL, -- 'google_ads', 'facebook_ads', etc.
    settings_json JSONB,
    encrypted_credentials TEXT,
    status VARCHAR(50) DEFAULT 'pending',
    last_synced_at TIMESTAMP WITH TIME ZONE
);

CREATE TABLE dashboards (
    id SERIAL PRIMARY KEY,
    project_id INTEGER REFERENCES projects(id) ON DELETE CASCADE,
    name VARCHAR(255) NOT NULL,
    config_json JSONB
);
Функциональные возможности
Диаграмма вариантов использования
Актор: Маркетолог

Варианты использования:

Авторизоваться в системе

Создать/выбрать проект

Подключить источник данных (интеграцию)

Создавать и настраивать дашборды

Формировать отчеты по метрикам (ROMI, CAC, LTV)

Анализировать воронку продаж

Проводить когортный анализ

Настраивать модели атрибуции

Актор: Руководитель

Варианты использования:

Авторизоваться в системе

Просматривать сводные дашборды

Отслеживать ключевые KPI

Фильтровать отчеты по датам и сегментам

Актор: Администратор

Варианты использования:

Авторизоваться в панели администратора

Управлять учетными записями пользователей

Мониторить состояние интеграций и системных сервисов

Управлять глобальными настройками системы

User-flow диаграммы
User Flow (Маркетолог - добавление интеграции и создание отчета):

Вход: Маркетолог входит в систему, используя email и пароль.

Выбор проекта: Выбирает рабочий проект из списка.

Переход к интеграциям: В меню навигации выбирает раздел "Интеграции".

Добавление интеграции: Нажимает кнопку "Добавить источник", выбирает "Google Ads".

Авторизация: Перенаправляется на страницу OAuth Google для предоставления разрешений.

Настройка: Выбирает рекламный аккаунт и задает расписание синхронизации.

Синхронизация: Система начинает фоновый процесс сбора данных.

Создание дашборда: Переходит в раздел "Дашборды", создает новый дашборд.

Добавление виджета: Добавляет виджет "ROMI по кампаниям", выбирает период и сохраняет дашборд.

User Flow (Руководитель - просмотр KPI):

Вход: Руководитель входит в систему.

Главный дашборд: Попадает на главный дашборд с ключевыми KPI (общая выручка, затраты, ROMI).

Фильтрация: Использует фильтр по датам, чтобы посмотреть данные за последний квартал.

Выход: Завершает сессию.

Детали реализации
UML-диаграммы
Для детального понимания реализации ПС могут быть использованы следующие UML-диаграммы:

Диаграмма компонентов: Визуализирует архитектуру на уровне компонентов (веб-клиент, API-шлюз, микросервисы, базы данных), показывая зависимости между ними.

Диаграмма последовательности (Sequence Diagram) для процесса "Сбор данных":

Scheduler периодически отправляет сообщение в RabbitMQ.

Integration Service получает сообщение и извлекает учетные данные из PostgreSQL.

Integration Service отправляет запрос к External API (например, Google Ads API).

External API возвращает сырые данные.

Integration Service отправляет сырые данные в другую очередь RabbitMQ.

ETL Service забирает данные из очереди.

ETL Service трансформирует данные и загружает их в ClickHouse.

Диаграмма классов: Описывает основные модели данных (User, Project, Integration, Dashboard) и их взаимосвязи, атрибуты и методы.

Спецификация API
API сервера спроектирован в соответствии с принципами REST и описан с использованием спецификации OpenAPI 3.0. Полная спецификация доступна по ссылке: https://api.analyticflow.com/v1/swagger.json

Примеры ключевых эндпоинтов:

POST /api/v1/auth/token - Аутентификация пользователя и получение JWT-токенов.

GET /api/v1/projects/{project_id}/integrations - Получение списка подключенных интеграций для проекта.

POST /api/v1/projects/{project_id}/integrations - Создание новой интеграции.

GET /api/v1/projects/{project_id}/reports/kpi - Получение ключевых метрик для отчета.

Request Body:

JSON

{
  "date_from": "2025-01-01",
  "date_to": "2025-03-31",
  "metrics": ["romi", "cac", "revenue"],
  "group_by": "campaign_name"
}
GET /api/v1/projects/{project_id}/dashboards/{dashboard_id} - Получение конфигурации дашборда.

Безопасность
Аутентификация: Для аутентификации используется стандарт JWT. После успешного входа пользователь получает access_token (короткоживущий, ~15 минут) и refresh_token (долгоживущий, ~30 дней). access_token используется для доступа к защищенным ресурсам и передается в заголовке Authorization: Bearer <token>.

Python

# Пример middleware в Django для проверки токена
from rest_framework_simplejwt.authentication import JWTAuthentication

class CustomApiAuthentication(JWTAuthentication):
    def authenticate(self, request):
        # ... логика проверки токена
        return super().authenticate(request)
Авторизация: Реализована ролевая модель доступа (RBAC). Доступ к данным строго сегментирован по project_id. Каждый запрос к API проходит через middleware, который проверяет, имеет ли аутентифицированный пользователь доступ к запрашиваемому проекту.

Python

# Пример проверки прав в view
class ReportViewSet(viewsets.ViewSet):
    permission_classes = [IsAuthenticated, IsProjectMember]

    def list(self, request, project_pk=None):
        project = self.get_project(project_pk) # Проверяет права доступа внутри
        # ... логика формирования отчета
Защита данных: Все чувствительные данные (учетные данные для интеграций, API-ключи) хранятся в зашифрованном виде в базе данных с использованием симметричного шифрования (AES-256). Весь трафик между клиентом и сервером шифруется с помощью TLS (HTTPS).

Оценка качества кода
Качество кода обеспечивается за счет следующих практик и инструментов:

Статический анализ: Используются линтеры (Pylint, Flake8 для Python; ESLint для JavaScript) и форматеры (Black, Prettier) для поддержания единого стиля кода.

Code Review: Обязательная проверка кода (pull requests) перед слиянием в основную ветку.

Метрики качества:

Покрытие тестами (Test Coverage): Целевой показатель > 85% для unit-тестов. Измеряется с помощью pytest-cov.

Цикломатическая сложность: Отслеживается, чтобы функции оставались простыми и читаемыми (цель < 10).

Дублирование кода: Минимизируется за счет вынесения общей логики в переиспользуемые функции и классы.

Тестирование
Unit-тесты
Представлены 5 примеров unit-тестов для серверной части с использованием pytest.

Тест расчета метрики ROMI: Проверяет корректность математической формулы.

Python

from analytics_service.metrics import calculate_romi

def test_calculate_romi():
    # ROMI = (Revenue - Cost) / Cost
    assert calculate_romi(revenue=1500, cost=1000) == 0.5
    assert calculate_romi(revenue=1000, cost=1500) == -1/3
    assert calculate_romi(revenue=1000, cost=0) == 0 # Деление на ноль
Тест прав доступа: Проверяет, что пользователь не может получить доступ к чужому проекту.

Python

def test_user_cannot_access_foreign_project(api_client, user_a, user_b, project_b):
    api_client.force_authenticate(user=user_a)
    response = api_client.get(f'/api/v1/projects/{project_b.id}/reports/kpi')
    assert response.status_code == 403 # Forbidden
Тест валидации данных интеграции: Проверяет корректность формата входных данных.

Python

from integrations_service.serializers import GoogleAdsSerializer

def test_google_ads_serializer_validation():
    # Неверные данные
    serializer = GoogleAdsSerializer(data={'customer_id': 'invalid-id'})
    assert not serializer.is_valid()
    # Корректные данные
    serializer = GoogleAdsSerializer(data={'customer_id': '123-456-7890'})
    assert serializer.is_valid()
Тест ETL-трансформации: Проверяет функцию очистки данных.

Python

from etl_service.transformers import clean_campaign_name

def test_clean_campaign_name():
    raw_name = " (Sale) My_Campaign_2025 | RU "
    expected = "sale_my_campaign_2025_ru"
    assert clean_campaign_name(raw_name) == expected
Тест модели пользователя: Проверяет, что пароль корректно хешируется.

Python

from django.contrib.auth.models import User

def test_user_password_is_hashed():
    user = User.objects.create_user(username='test', password='plain_password')
    assert user.check_password('plain_password')
    assert not user.password == 'plain_password'
Интеграционные тесты
Интеграционные тесты проверяют взаимодействие между несколькими компонентами системы. Они запускаются в изолированном окружении с использованием docker-compose.

Сквозной тест сбора и отображения данных:

Тестовый скрипт через API создает проект и пользователя.

Добавляется интеграция, которая подключается к mock-серверу внешнего API.

Запускается задача сбора данных.

Тест проверяет, что данные появились в тестовой базе ClickHouse.

Тест делает запрос к API отчетов и проверяет, что расчетная метрика совпадает с ожидаемой на основе mock-данных.

Тест процесса аутентификации:

Тест отправляет запрос на эндпоинт /api/v1/auth/token с валидными данными и проверяет получение JWT.

Затем отправляет запрос к защищенному эндпоинту с полученным токеном и ожидает статус 200.

Отправляет запрос с невалидным/просроченным токеном и ожидает статус 401.

Установка и запуск
Манифесты для сборки docker образов
Dockerfile для сервера (Django):

Dockerfile

# Use an official Python runtime as a parent image
FROM python:3.10-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Set work directory
WORKDIR /usr/src/app

# Install dependencies
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy project
COPY . .

# Expose port
EXPOSE 8000

# Run gunicorn
CMD ["gunicorn", "analyticflow.wsgi:application", "--bind", "0.0.0.0:8000"]
Dockerfile для клиента (React):

Dockerfile

# build environment
FROM node:18-alpine as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . ./
RUN npm run build

# production environment
FROM nginx:stable-alpine
COPY --from=build /app/build /usr/share/nginx/html
# new
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
Манифесты для развертывания k8s кластера
Deployment для сервиса аналитики (analytics-service-deployment.yaml):

YAML

apiVersion: apps/v1
kind: Deployment
metadata:
  name: analytics-service
  labels:
    app: analytics-service
spec:
  replicas: 2
  selector:
    matchLabels:
      app: analytics-service
  template:
    metadata:
      labels:
        app: analytics-service
    spec:
      containers:
      - name: analytics-service
        image: your-registry/analyticflow-server:latest
        ports:
        - containerPort: 8000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: postgres-secret
              key: url
        - name: CLICKHOUSE_HOST
          value: "clickhouse-service"
Service для сервиса аналитики (analytics-service-service.yaml):

YAML

apiVersion: v1
kind: Service
metadata:
  name: analytics-service
spec:
  selector:
    app: analytics-service
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8000
  type: ClusterIP
Лицензия
Этот проект лицензирован по лицензии MIT - подробности представлены в файле LICENSE.

Контакты
Автор: your.email@example.com
