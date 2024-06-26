# Тестовое задание для отбора на Летнюю ИТ-школу КРОК по разработке

## Условие задания
Будучи тимлидом команды разработки, вы получили от менеджера проекта задачу повысить скорость разработки. Звучит, как начало плохого анекдота, но, тем не менее, решение вам все же нужно найти. В ходе размышлений и изучений различного внешнего опыта других команд разработки вы решили попробовать инструменты геймификации. То есть применить техники и подходы игрового характера с целью повышения вовлеченности команды в решение задач.

Вами была придумана рейтинговая таблица самых активных контрибьютеров за спринт. Что это значит в теории: по окончании итерации (4 рабочие недели) выгружается список коммитов, сделанных в релизную ветку продукта, и на его основе вычисляются трое самых активных разработчиков, сделавших наибольшее количество коммитов. В зависимости от занятого места, разработчик получает определенное количество внутренней валюты вашей компании, которую он впоследствии может обменять на какие-то товары из внутреннего магазина.

На практике вы видите решение следующим образом: на следующий день после окончания спринта в 00:00 запускается автоматическая процедура, которая забирает файл с данными о коммитах в релизную ветку, сделанных в период спринта, после чего выполняется поиск 3-х самых активных контрибьютеров. Имена найденных разработчиков записываются в файл, который впоследствии отправляется вам на почту.

В рамках практической реализации данной задачи вам необходимо разработать процедуру формирование отчета “Топ-3 контрибьютера”. Данная процедура принимает на вход текстовый файл (commits.txt), содержащий данные о коммитах (построчно). Каждая строка содержит сведения о коммите в релизную ветку в формате: “_<Имя пользователя> <Сокращенный хэш коммита> <Дата и время коммита>_”.
Например: AIvanov 25ec001 2024-04-24T13:56:39.492

К данным предъявляются следующие требования:
- имя пользователя может содержать латинские символы в любом регистре, цифры (но не начинаться с них), а также символ "_";
- сокращенный хэш коммита представляет из себя строку в нижнем регистре, состояющую из 7 символов: букв латинского алфавита, а также цифр;
- дата и время коммита в формате YYYY-MM-ddTHH:mm:ss.

В результате работы процедура формирует новый файл (result.txt), содержащий информацию об именах 3-х самых активных пользователей по одному в каждой строке в порядке убывания места в рейтинге. Пример содержимого файла:
AIvanov
AKalinina
CodeKiller777

Ручной ввод пути к файлу (через консоль, через правку переменной в коде и т.д.) недопустим. Необходимость любых ручных действий с файлами в процессе работы программы будут обнулять решение.

## Автор решения
Дмитриев Артем Вадимович

## Описание реализации
Небольшая оговорка: так как в задании в разных местах используется разный формат даты (как 2024-04-24T13:56:39.492 в примере, 
так и YYYY-MM-ddTHH:mm:ss в описании требований к данным), то я решил использовать второй формат, так как он более читаемый и понятный.

**Пакет exceptions**:
- **ReadingException** - исключение, возникающее при ошибке чтения файла
- **WritingException** - исключение, возникающее при ошибке записи файла

**Пакет reader**:

- **CommitsFileReader** - класс, используемый для чтения содержимого из файла по пути **src/main/resources/commits.txt**. Внутри файла один метод:
```java
public static Map<String, Integer> readFromResources();
```
Метод возвращает Map, где ключ - имя пользователя, а значение - количество коммитов. 
В случае, если одна из строк невалидна, в консоль выводится сообщение об ошибке и строка пропускается.
В случае ошибки чтения из файла выбрасывается исключение **ReadingException**.

- **CommitValidator** - класс, используемый для валидации строки с коммитом. Внутри файла один публичный метод:
```java
public static boolean validateLine(String line);
```
Метод возвращает true, если строка с коммитом валидна, и false в противном случае.

**Пакет solver**:
- **CalculateTop** - класс, используемый для нахождения топ-3 контрибьютеров. Внутри файла один метод:
```java
public static List<String> calculateTopContributors(Map<String, Integer> contributors);
```
Метод возвращает список имен пользователей, которые сделали наибольшее количество коммитов в порядке убывания.

**Пакет writer**:
- **CommitsFileWriter** - класс, используемый для записи результатов в файл по пути **src/main/resources/result.txt**. Внутри файла один метод:
```java
public static void writeToFile(List<String> topContributors);
```
Метод записывает в файл список имен пользователей, которые сделали наибольшее количество коммитов в порядке убывания. 
В случае ошибки записи в файл выбрасывается исключение **WritingException**.

## Инструкция по сборке и запуску решения
Выполнить в корне проекта следующие команды:
```
mvn clean install
java -jar target/school2024-test-task4-1.0.jar
```
