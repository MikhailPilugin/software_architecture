# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
-->
```plantuml
@startuml
' !include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
!include <C4/C4_Container.puml>

Person(user, "Пользователь")

System(conference_site, "Сайт конференции", "Веб-сайт для работы с конференциями")

Rel(user, conference_site, "Регистрация, просмотр/изменение персональной информации, поиск пользователей")
Rel(user, conference_site, "Создание доклада, получние списка всех докладов")
Rel(user, conference_site, "Добавление доклада в конференцию, получние списка докладов в конференции")


@enduml
```
## Назначение систем
|Система| Описание|
|-------|---------|
| Сайт конференции | Веб-интерфейс, обеспечивающий доступ к информации о конференциях, докладах и пользователях. Бэкенд сервиса реализован в виде микросервисной архитектуры |
