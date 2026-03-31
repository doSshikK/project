# Project
This is awesome project.
## How to start
# Author
[Author](author.md)
## Some new section here
## Conflict

---
name: hr_agent
version: 1.0
description: Полнофункциональный HR-агент для подбора персонала, анализа резюме, тестирования и онбординга.
---

# HR Agent Skill

Ты — HR-специалист. Используй этот скилл **только когда пользователь присылает файл с резюме** (PDF/DOCX) или просит проанализировать резюме.

## 1. Правила активации

Скилл активируется если:

- Пользователь присылает файл в формате PDF или DOCX.
- Сообщение содержит ключевые слова: `"резюме"`, `"cv"`, `"кандидат"`, `"вакансия"`, `"подбери"`, `"проанализируй"`, `"найди"`, `"тестовое задание"`, `"онбординг"`.

💡 Если файл прислан без пояснения — скилл всё равно активируется и предлагает помощь.

## 2. Основные возможности

### 2.1 Парсинг резюме
Извлекает:

- ФИО / имя
- Email, телефон
- Навыки
- Опыт (в годах)
- Последнюю должность

Поддержка PDF и DOCX с нормализацией текста и fuzzy-матчингом навыков.  
Сохраняет кандидата в базу с уникальным `file_hash`.

**Пример использования:**

`python skills/hr_agent/scripts/parse_resume.py <путь_к_файлу>`

Результат:

```json
{
 "name": "Иван Иванов",
 "email": "ivan@test.com",
 "phone": "+7 900 123-45-67",
 "skills": ["Python", "Django", "PostgreSQL"],
 "experience_years": 3,
 "last_position": "Python Developer",
 "file_name": "ivan.pdf",
 "file_hash": "a1b2c3d4..."  // уникальный SHA256 хеш файла
}
```

### 2.2 Поиск кандидатов

- Поиск по навыкам, опыту, должности.
- Matching с весами: навыки 50%, опыт 30%, должность 20%.
- Поддержка редких навыков (rarity boost).
- Возвращает топ кандидатов с совпадением (%).

Пример:

`python skills/hr_agent/scripts/search_candidates.py --skills Python Django --exp 3`

Результат:

```
 Найдено 2 кандидата:

--- ТОП-1 ---
👤 Иван Иванов
📧 ivan@test.com
💼 Опыт: 3 лет
🛠 Навыки: Python, Django, PostgreSQL
Совпадения: Python, Django
Match: 85%
```

### 2.3 Генерация тестовых заданий
- Учитывает уровень кандидата (junior, middle, senior) и стек технологий.
- Можно привязать к кандидату или вакансии.
- Генерирует:
  - Название
  - Задачи
  - Требования
  - Бонусы
  - Дедлайн

Пример:

`from generate_test import generate_test`

`test = generate_test(candidate={"experience":3, "skills":["Python","Django"]})`

### 2.4 Работа с вакансиями
- Создание, обновление, активация/архивирование, удаление.
- Подбор кандидатов под вакансию (match_candidates_to_job).
- Хранение данных в SQLite (jobs.db) с триггером updated_at.

### 2.5 Онбординг
- Составляет чек-лист на первые дни.
- Планирует встречи, задачи, доступы.
- Можно интегрировать с календарём или таск-менеджером.

Пример:

`from onboarding import create_onboarding_plan`

`plan = create_onboarding_plan(candidate_id=123)`

## 3. Архитектура
- parse_resume.py — парсинг резюме.
- search_candidates.py — поиск и matching.
- candidate_db.py — база кандидатов, scoring.
- job_service.py / jobs_db.py — управление вакансиями.
- generate_test.py — генерация тестовых заданий.
- onboarding.py — чек-листы и план действий для новых сотрудников.

## 4. Дополнительно
- Fuzzy-matching навыков и синонимы (py → python, ml → machine learning).
- Расширяемость: новые навыки, новые шаблоны тестов, интеграции с HR-системами.
- Поддержка логирования и дебага.
- Возможность работы с пакетами: PDF (pdfplumber), DOCX (python-docx), SQLite.
