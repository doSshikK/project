# Project
This is awesome project.
## How to start
# Author
[Author](author.md)
## Some new section here
## Conflict

## 1. Парсинг резюме
**Сценарий:** Извлечение структурированных данных из резюме кандидата

Пользователь: [отправляет файл resume.pdf]

Агент: Читаю файл resume.pdf...

      > parse_resume("resume.pdf")
      > extract_cv_data("resume.pdf")

Агент: Извлек данные:

      - ФИО: Иванов Иван Иванович
      - Email: ivanov@email.com
      - Телефон: +7 (999) 123-45-67
      - Навыки: Python, Django, SQL, Docker
      - Опыт работы: 5 лет
      - Образование: МГУ, прикладная математика
      - Текущее место работы: Яндекс

      Сохранить кандидата в базу? (yes/no)

[Пользователь: yes]

Агент: Кандидат сохранен с ID: CAND-001
