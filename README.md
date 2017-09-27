# metar

Немного распишу что сделано в этом проекте.
1. Разработан класс parser (parser.php). В данном классе реализованы возможности для получения данных о погоде (metar) с сервера aviationweather.gov, обработки этой информации через метод getCurrentTempAndTime(). 
Далее для вывода информации сделан шаблон страницы на основе bootstrap - templatemain.php.
В файле index.php реализован сценарий для получения данных о погоде с помощью объекта класса parser, формирование блока для вывода в переменной currentTempBlock и вывод этой информации пользователю через шаблон.
2. Чтобы накапливать данные в класс parser добавлен функционал для работы с базой данных (структура бд описана в файле metar.sql): метод getLastValueTempFromDB - выдает последнее записанное в базу значение температуры, insertValueToDB - осуществляет вставку строки с температурой и временем в базу данных.
Для сбора данных реализован демон (daemon.php), который запускается из консоли и начинает бесконечный цикл с опросом сервера через каждые десять минут. Демон сравнивает текущие данные с сервера с последними записанными в базу данных, если они более новые, то записывает их в базу. Таким образом идет накопление данных.
3. Реализован вывод суточного графика температуры в файле index.php через шаблон и использование компонента графика (файлы chart.css и chart.js) от shieldui.com. 
Данный блок фармируется в переменной chartDay в файле index.php, которая затем выводится в теле шаблона. График формируется на основе записей за последние 24 часа с текущего момента из базы данных. 
В классе parser дополнительно реализован метод для получения значений температуры между начальной и конечной датой - getTempValues.
4. Кроме этого реализовано два дополнительных компонента для ввода даты начала и конца формирования графика - datetimepicker.
В составе класса parser для вычисления значений мин, макс и средней температуры реализован метод getTempMaxMinAvg.
Блок для вывода информации по данному пункту формируется в переменных resizeBlock (ввод данных и вывод min, max, avg) и resizeBlock2 (график) файла index.php, которая затем выводиться через шаблон.

Настройки проекта хранятся в файле config.php. Для отладки еще использовал файл logging.php.

Методы класса я реализовал без проверок на содержание переменных и соответствие типам что не очень хорошо конечно.