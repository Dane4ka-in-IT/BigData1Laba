# Лабораторная работа №1: Нормализация данных в схему «Снежинка»

## Описание проекта
В рамках работы исходные сырые данные о продажах зоотоваров были импортированы в базу данных PostgreSQL и нормализованы в аналитическую схему "Снежинка".

Схема включает:
- Таблицу фактов: `fct_transactions`
- Таблицы измерений: `dim_buyers`, `dim_vendors`, `dim_products`, `dim_stores`
- Таблицы справочников: `ref_countries`, `ref_brands`, `ref_categories`, `ref_pet_categories`, `ref_pet_types`, `ref_pet_breeds`, `ref_materials`

## Как запустить проект

1. Склонируйте репозиторий.
2. Запустите контейнер с базой данных (все скрипты DDL, DML и импорт CSV выполнятся автоматически):
   ```bash
   docker-compose up -d
   ```
## Проверка результатов импорта

Чтобы проверить корректность работы схемы, вы можете выполнить следующие SQL-запросы внутри контейнера.

### 1. Вход в консоль PostgreSQL
```bash
docker exec -it petstore_dwh_db psql -U dw_admin -d snowflake_db
```
### 2. Проверка количества строк
```sql
SELECT COUNT(*) FROM fct_transactions;
```
### 3. Проверка распределения данных по справочникам и измерениям
```sql
SELECT 'countries'  AS table_name, COUNT(*) FROM ref_countries
UNION ALL SELECT 'brands',         COUNT(*) FROM ref_brands
UNION ALL SELECT 'categories',     COUNT(*) FROM ref_categories
UNION ALL SELECT 'buyers',         COUNT(*) FROM dim_buyers
UNION ALL SELECT 'vendors',        COUNT(*) FROM dim_vendors
UNION ALL SELECT 'products',       COUNT(*) FROM dim_products
UNION ALL SELECT 'stores',         COUNT(*) FROM dim_stores
UNION ALL SELECT 'suppliers',      COUNT(*) FROM dim_suppliers;
```