[![Join the chat at https://gitter.im/EvilBeaver/oscript-library](https://badges.gitter.im/EvilBeaver/oscript-library.svg)](https://gitter.im/EvilBeaver/oscript-library?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![GitHub release](https://img.shields.io/github/release/artbear/1testrunner.svg)](https://github.com/artbear/1testrunner/releases) [![Build Status](http://build.oscript.io/buildStatus/icon?job=oscript-library/1testrunner/develop)](http://build.oscript.io/job/oscript-library/job/1testrunner/job/develop/) [![Build status](https://ci.appveyor.com/api/projects/status/7sgdu30u1yqbot4m?svg=true)](https://ci.appveyor.com/project/artbear/1testrunner) [![Build Status](https://travis-ci.org/artbear/1testrunner.svg)](https://travis-ci.org/artbear/1testrunner)

Организовано приемочное тестирование, аналогичное тестированию 1C в проекте [xUnitFor1C](https://github.com/xDrivenDevelopment/xUnitFor1C/wiki)

Основные принципы работы с тестами для скриптов OneScript описаны в [официальной документации OneScript](http://oscript.io/docs/page/testing)

# Использование тестирования (выдержка из документации OneScript)

## Пример запуска всех приемочных тестов ###

Проверить все файлы текущего каталога из командной строки (с паузой, если есть упавшие тесты):

    cmd /c C:\Projects\1script\tests\start-all.cmd .

Проверить все файлы текущего каталога из командной строки (без паузы, если есть упавшие тесты):

    "C:\Program Files (x86)\OneScript\oscript.exe" "ПутьStart\testrunner.os" -runall "ТекущийКаталог" xddReportPath "ТекущийКаталог"

или

    cmd /c C:\Projects\1script\tests\start-all.cmd . notpause

## Запуск тестов ###

### Формат командной строки:

    oscript tests\testrunner.os [-command] testfile|testdir [test-id|test-number] [-option [optionData]]


### Виды команд

* `-show` - вывод доступных тестов с именами тестов и номерами тестов по порядку объявления
* `-run` - прогон всех тестов из файла теста или одного конкретного теста, уточненного по номеру или наименованию
* `-runall` - прогон всех тестов из каталога, в т.ч. и из вложенных каталогов

### Виды режимов

* `xddReportPath` - формировать отчет тестирования в формате junit-xml
* * [optionData] - полный или относительный путь к каталогу, где формировать файл *.xml

### Примеры:

* `oscript tests\testrunner.os -show testfile` - вывод списка тестов
* `oscript tests\testrunner.os testfile` или `oscript tests\testrunner.os -run testfile` - запуск всех тестов из файла
* `oscript tests\testrunner.os -run testfile 5` или `oscript tests\testrunner.os testfile 5` - запуск теста №5
* `oscript tests\testrunner.os -run testfile "Тест1"` или `oscript tests\testrunner.os testfile "Тест1"`- запуск теста с именем Тест1

* `oscript tests\testrunner.os -runall tests` - запуск всех тестов из каталога tests
* `oscript tests\testrunner.os -runall tests xddReportPath .` - запуск всех тестов из каталога tests и формирование отчета тестирования в формате  junit-xml

### Формат скриптов-тестов

Тесты находятся в каталоге `tests`

Пример скрипта-теста находится в `tests\example-test.os` :

```bsl
#Использовать asserts

Перем юТест;

// основной метод для тестирования
Функция ПолучитьСписокТестов(ЮнитТестирование) Экспорт

	юТест = ЮнитТестирование;

	ВсеТесты = Новый Массив;

	ВсеТесты.Добавить("ТестДолжен_ПроверитьВерсию");

	Возврат ВсеТесты;
КонецФункции

Процедура ТестДолжен_ПроверитьВерсию() Экспорт
	Утверждения.ПроверитьРавенство("0.1", Версия());
КонецПроцедуры

Функция Версия() Экспорт
	Возврат "0.1";
КонецФункции
```

### Механизм работы с временными файлами

В testrunner.os встроен механизм работы с временными файлами.
Удобен для автосоздания и автоудаления файлов после выполнения тестов.
Вызывать через юТест.

Методы:

* **ИмяВременногоФайла**() - возвращается имя временного файла и имя фиксируется для дальнейшего удаления
* **УдалитьВременныеФайлы**() - удаляются все зарегистрированные ранее временные файлы
* * Удобно этот метод использовать в 'ПослеЗапускаТеста'

Пример использования методов находятся в тесте temp-files.os

## Запуск тестирования из Notepad++

### Для прогона тестов из текущего открытого файла скрипта

в Notepad++ (в т.ч. и для плагина NppExec) можно использовать следующую команду:

    cmd.exe /c C:\Projects\1script\tests\start.cmd "$(FULL_CURRENT_PATH)"

или

    "C:\Program Files (x86)\OneScript\oscript.exe" "$(CURRENT_DIRECTORY)\testrunner.os" -run "$(FULL_CURRENT_PATH)"

В случае ошибок в тестах/файле будет выдано окно консоли с описанием ошибки.

### Пример запуска всех приемочных тестов ###

    "C:\Program Files (x86)\OneScript\oscript.exe" "$(CURRENT_DIRECTORY)\testrunner.os" -runall "$(CURRENT_DIRECTORY)"
