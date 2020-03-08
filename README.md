# Дипломный проект профессии «Тестировщик»

Дипломный проект представляет собой автоматизацию тестирования 
комплексного сервиса, взаимодействующего с СУБД и API Банка.

### План автоматизации 
 [Plan.md](https://github.com/shvedcate/Diplom/blob/master/Plan.md)
 
### Отчётные документы по итогам тестирования

**Отчёт о проведённом тестировании**

[Report.md]()

### Отчётные документы по итогам автоматизации

[Summary.md]()

## Запуск приложения

Для запуска необходимы следующие инструменты:

* Git
* Java Jre 1.8
* браузер Google Chrome
* Docker (Docker Toolbox), либо Node.js 
* СУБД: Mysql 8 или Postgres 12.

1. Запустить Docker Toolbox (Если установлен Docker, то в файле application.properties
нужно вместо ip хоста написать localhost)
1. Загрузить контейнеры mysql, postgres и образ платежного шлюза nodejs в терминале IDEA командой 
          
    ````
    docker-compose up
    ````
 
1. В третьем терминале в папке artifacts запустить SUT командой

   - для конфигурации с базой данный MySql: 
  
      ````
      java -Dspring.datasource.url=jdbc:mysql://192.168.99.100:3306/app -jar aqa-shop.jar
      ````
            
   - для конфигурации с базой данных PostgreSQL:
  
       ````
       java -Dspring.datasource.url=jdbc:postgresql://192.168.99.100:5432/app -jar aqa-shop.jar
       ```` 
            
1. В браузере открыть SUT в окне с адресом 
      ````
      localhost:8080
      ````
1. Запустить автотесты командой 

   -  для конфигурации с MySql
 
      ````
      gradlew test -Dspring.datasource.url=jdbc:mysql://192.168.99.100:3306/app
      ````
            
   - для конфигурации с postgresql
 
      ````
      gradlew test -Dspring.datasource.url=jdbc:postgresql://192.168.99.100:5432/app
      ````
1. Остановить SUT командой CTRL + C

1. Остановить контейнеры командой CTRL + C и после очистить контейнеры командой

      ````
      docker-compose down
      ````     

## Описание приложения

Приложение представляет из себя веб-сервис по покупке тура.
Приложение предлагает купить тур по определённой цене с помощью двух способов:

1. Обычная оплата по дебетовой карте
1. Уникальная технология: выдача кредита по данным банковской карты

Само приложение не обрабатывает данные по картам, а пересылает их 
банковским сервисам:

* сервису платежей (далее - Payment Gate)
* кредитному сервису (далее - Credit Gate)
Приложение должно в собственной СУБД сохранять информацию о том, 
каким способом был совершён платёж и успешно ли он был совершён 
(при этом данные карт сохранять не допускается).

В файле application.properties:

* учётные данные 
* url-адреса банковских сервисов

Заявлена поддержка двух **СУБД**:

* MySQL
* PostgreSQL

Симулятор банковских сервисов написан на Node.js. 
Симулятор расположен в каталоге gate-simulator. 
Для запуска необходимо перейти в этот каталог.
Запускается на порту 9999.

Симулятор позволяет для заданного набора карт генерировать 
предопределённые ответы.

Набор карт представлен в формате JSON в файле data.json.

Разработчики сделали один сервис, симулирующий и Payment Gate, и Credit Gate.






