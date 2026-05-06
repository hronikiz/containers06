# Лабораторная работа №6: Работа с Docker и взаимодействие контейнеров nginx и php-fpm

## Название лабораторной работы
Управление взаимодействием нескольких контейнеров (nginx + php-fpm)

---

## Цель работы
Получить навыки создания и настройки взаимодействия нескольких Docker-контейнеров в одной сети без использования docker-compose.

---

## Задание
Создать PHP приложение на базе двух контейнеров:
- nginx
- php-fpm

Использование docker-compose запрещено.

Необходимо:
- создать сеть internal
- создать контейнер backend (php-fpm)
- создать контейнер frontend (nginx)
- настроить взаимодействие контейнеров
- проверить работу сайта через браузер

---

## Выполнение работы

### 1. Создание структуры проекта
Создана директория `containers06`:

containers06/

├── mounts/

│ └── site/

├── nginx/

│ └── default.conf

└── README.md

---

### 2. Создание Docker сети
Создана сеть internal для взаимодействия контейнеров.
<img width="605" height="45" alt="Снимок экрана 2026-05-06 152704" src="https://github.com/user-attachments/assets/0ae50e4b-36fd-485d-99bb-a5338502f004" />

<img width="979" height="446" alt="Снимок экрана 2026-05-06 152729" src="https://github.com/user-attachments/assets/e21d86ac-6147-4fee-9d00-77cfd9ea620e" />

---

### 3. Запуск backend (php-fpm)
Создан контейнер backend на базе php-fpm, подключённый к сети internal, с примонтированной директорией сайта в /var/www/html.
<img width="1919" height="1079" alt="Снимок экрана 2026-05-06 153208" src="https://github.com/user-attachments/assets/6ea2fe95-8299-4c72-a1e2-344d7d068963" />

<img width="1919" height="1079" alt="Снимок экрана 2026-05-06 153213" src="https://github.com/user-attachments/assets/3a6536eb-71be-4d99-b194-9b1cc6611fd2" />

<img width="478" height="131" alt="Снимок экрана 2026-05-06 152712" src="https://github.com/user-attachments/assets/67f7b984-16a6-4897-bd30-41cb1a00177b" />

---

### 4. Запуск frontend (nginx)
Создан контейнер frontend на базе nginx, подключённый к сети internal, с:
- примонтированной директорией сайта
- кастомной конфигурацией nginx
- пробросом порта 80 на хост
<img width="1624" height="297" alt="image" src="https://github.com/user-attachments/assets/5eff153a-3bc4-4d8e-a8a5-0ea2db346ea9" />

---

### 5. Проверка работы
Сайт открыт в браузере по адресу:

http://localhost<img width="1919" height="1079" alt="Снимок экрана 2026-05-06 153208" src="https://github.com/user-attachments/assets/7b843c15-e2fe-406b-a617-fbe7f35292b2" />

<img width="1919" height="1079" alt="Снимок экрана 2026-05-06 153213" src="https://github.com/user-attachments/assets/b575d11f-3fbd-4b5c-ab3f-1aa5b5dcea1c" />

<img width="1919" height="1079" alt="Снимок экрана 2026-05-06 153208" src="https://github.com/user-attachments/assets/0f46db83-8c3d-436d-afb0-cecc6fb698a7" />

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/c22b1077-4976-4dda-9783-373ca90d36fe" />

При корректной настройке отображается PHP-страница.

---

## Ответы на вопросы

### 1. Как контейнеры взаимодействуют друг с другом?
Контейнеры взаимодействуют через общую Docker-сеть internal.  
Nginx перенаправляет PHP-запросы в backend (php-fpm) по имени контейнера backend.

---

### 2. Как контейнеры видят друг друга в сети internal?
Docker предоставляет встроенный DNS внутри сети.  
Контейнеры обращаются друг к другу по имени сервиса:
backend:9000

---

### 3. Почему нужно переопределять конфигурацию nginx?
Потому что стандартный nginx не умеет обрабатывать PHP-файлы через PHP-FPM.  
Необходимо вручную указать обработку .php файлов и передачу запросов в backend.

---

## Вывод
В ходе работы изучено взаимодействие Docker-контейнеров в общей сети. Реализована схема из двух контейнеров (ng<img width="605" height="45" alt="Снимок экрана 2026-05-06 152704" src="https://github.com/user-attachments/assets/3c194d5d-2fd8-4429-aab6-e3da864ba360" />
