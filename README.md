**Отчет выполнения: **
**1. Очистка Docker-образов:**
Успешно остановлены и удалены все контейнеры.
Успешно удалены все Docker-образы.

**2. Сборка и запуск Docker-контейнера:**
Успешно построен и запущен контейнер с наименованием api.py на порту 5000.

**3. Создание директории для отчетов, виртуального окружения, загрузка шаблона Trivy, установка Semgrep:**
Успешно создана директория reports.
Успешно создано виртуальное окружение venv.
Успешно загружен шаблон Trivy в файл html.tpl.
Успешно установлен Semgrep в виртуальное окружение.

**4. Сканирование на безопасность с использованием Trivy:**
Успешно проведено сканирование контейнера api.py с использованием Trivy.
Сгенерирован отчет HTML в директории reports.
Сканирование завершено без ошибок (не обнаружено критических уязвимостей).

**5. Сканирование на безопасность с использованием Semgrep:**
Успешно проведено сканирование кода файла api.py с использованием Semgrep.
Сгенерирован отчет JUnit XML в директории reports.
Сканирование завершено без ошибок (не обнаружено уязвимостей).

**Уязвимость:**
Потенциальная уязвимость заключается в том, что контейнер и файл кода могут быть названы как api.py,
но при этом содержать уязвимый код. Это может включать в себя использование устаревших 
,иблиотек, возможные SQL-инъекции или другие уязвимости, которые могут быть использованы злоумышленниками для атак.




