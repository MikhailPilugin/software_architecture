# Компонентная архитектура
<!-- Состав и взаимосвязи компонентов системы между собой и внешними системами с указанием протоколов, ключевые технологии, используемые для реализации компонентов.
Диаграмма контейнеров C4 и текстовое описание. 
-->
## Компонентная диаграмма

```plantuml
@startuml
' !include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
!include <C4/C4_Container.puml>

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(user, "Пользователь")

System_Ext(web_site, "Клиентский веб-сайт", "HTML, CSS, JavaScript, React", "Веб-интерфейс")

System_Boundary(conference_site, "Сайт конференции") {
   'Container(web_site, "Клиентский веб-сайт", ")
   Container(client_service, "Сервис авторизации", "C++", "Сервис управления пользователями", $tags = "microService")    
   Container(post_service, "Сервис докладов", "C++", "Сервис управления докладами", $tags = "microService") 
   Container(blog_service, "Сервис конференций", "C++", "Сервис управления конференциями", $tags = "microService")   
   ContainerDb(db, "База данных", "MySQL", "Хранение данных о конференциях, докладах и пользователях", $tags = "storage")
   
}

Rel(admin, web_site, "Просмотр, добавление и редактирование информации о докладах, пользователях и их конференциях")
Rel(moderator, web_site, "Модерация докладов, пользователей и их конференций")
Rel(user, web_site, "Регистрация, просмотр докладов, редактирование конференции")

Rel(web_site, client_service, "Работа с пользователями", "localhost/account")
Rel(client_service, db, "INSERT/SELECT/UPDATE", "SQL")

Rel(web_site, post_service, "Работа с докладами", "localhost/report")
Rel(post_service, db, "INSERT/SELECT/UPDATE", "SQL")

Rel(web_site, blog_service, "Работа с конференциями ", "localhost/conference")
Rel(blog_service, db, "INSERT/SELECT/UPDATE", "SQL")

@enduml
```
## Список компонентов  

### Сервис авторизации
**API**:
-	Создание нового пользователя
      - входные параметры: login, пароль, имя, фамилия, email, обращение (г-н/г-жа)
      - выходные параметры: отсутствуют
-	Поиск пользователя по логину
     - входные параметры:  login
     - выходные параметры: имя, фамилия, email, обращение (г-н/г-жа)
-	Поиск пользователя по маске имени и фамилии
     - входные параметры: маска фамилии, маска имени
     - выходные параметры: login, имя, фамилия, email, обращение (г-н/г-жа)

### Сервис докладов
**API**:
- Создание доклада
  - Входные параметры: тема доклада, категория, аннотация и автор
  - Выходыне параметры: идентификатор доклада
- Получение доклада
  - Входные параметры: идентификатор доклада, автор
  - Выходыне параметры: тема доклада, категория и аннотация
- Получение списка всех докладов
  - Входные параметры: отсутствуют
  - Выходные параметры: массив докладов, где для каждого указаны его идентификатор, тема, категория, аннотация и автор

### Сервис конференций
**API**:
- Создание конференции
  - Входные параметры: название конференции, тематика конференции
  - Выходные параметры: отсутствуют
- Получение информации о конференции
  - Входные параметры: название конференции
  - Выходные параметры: тематика конференции, доклады на конференции, авторы докладов
- Добавление доклада в конференцию
  - Входные параметры: тема доклада, автор, конференция, содержание доклада, идентификатор доклада
  - Выходные параметры: идентификатор доклада в сервисе конференций
- Получение списка докладов в конференции
  - Входные параметры: конференция
  - Выходные параметры: массив с докладами (идентификатор, тема доклада, автор, конференция, содержание доклада)