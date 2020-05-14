# Gulag Exhibition Control System (GECS)

<img src="https://i.imgur.com/W8wbb4B.jpg" height="450">

## Что такое GECS?
Музейная техника, особенно сделанная без участия крупного капитала часто подводит. Ее не удобно обслуживать и следить за ней. Но как правило ей можно удаленно управлять.
GECS позволяет управлять экспозционной техникой. На данный момент это следующие устройства:

- Windows PC
- проекторы с поодержкой PJLink
- телевизоры Sharp
- Dataton Watchpax
- Raspberry PI на Raspbian
- сенсоры присутсвия или движения

GECS умеет:
- включать/выключать устройства вместе и по отдельности
- наблюдать за их состоянием (работает-сломалось)
- автоматически перезагружать если сломалось
- предоставлять смотрителям возможность чинить если они видят что что-то сломалось через Telegram bot. 

Разумеется, есть коммерческие решения, которые это все умеют (например Crestron). Но, во-первых, не всегда есть возможность платить деньги, а во-вторых, хочется иметь управление и возможность настройки в руках. 
Разумеется, есть open-source, который это все умеет (например OpenHab). Но для того, чтобы его качественно и полноценно его настроить нужно выучить язык OpenHab, а когда дела идут плохо, Java. Мы решили, что лучше сразу использовать Java.
## Как установить GECS?
GECS - программа, написанная на JAVA 8 SE. На данный момент она испытана только на Windows 10. Чтобы запустить ее нужно:
1. Установить [Java](https://www.java.com/ru/) если у вас он еще не установлен
2. Скачать [программу GECS]()
3. Распаковать программу и запустить ее, написав в командной строке
```
java -jar GECS.jar
```
4. Она запустится и отобразит конфигурацию оборудования Музея истории ГУЛАГа. Вряд ли вам это нужно. Для того, чтобы вы могли сделать то, что вам нужно необходимо записать вашу конфигурацию оборудования в настроечные файлы JSON
## Настраиваем JSON
Все настройки GECS хранятся в JSON файлах:
structure.json - информация об устройствах и их конфигурация
cronSchedule.json - информация о регулярном включении и выключении экспозиции
specialSchedule.json - информация о специальных днях включения и выключения экспозиции
views.json - информация о том, как отображать устройства в окне GECS
viewTypes.json - информация о том, как выглядят устройства
sources.json - информация о сенсорах и том, что они делают
bots.json - информация о телеграм-ботах
### Вносим конфигурацию экспозиции в structure.json
Структура, в которой находятся устройства

<img src="https://i.imgur.com/AWEZy5P.jpg" height="250">

**switchGroup** - группа включения - множество устройств и модулей, которые система включает и выключает вместе. Включение и выключение  можно задать по расписанию в файлах cronSchedule.json и specialSchedule.json. Группа отображается в интерфейсе в виде закладки. Включение, выключение и проверка текущего состояния всех устройств группы производится по кнопкам Switch all off, Switch all on и Update.

Как правило, группа испозльзуется для функционально разных помещений. Например, группа "экспозиция", "кафе", "библиотека".

**vismodule** - модуль отображения. Включает в себя одно *устройство выдачи сигнала* и множество *устройств отображения*. Если модуль отображения находится в группе включения, то включение устройств происходит по следующему алгоритму:
1. Одновременно включаются все устройства отображения. Если хотя бы одно устройство отбражения дол сбой, включение останавливается.
2. Включается устройство выдачи сигнала.

Выключение происходит в обратном порядке.

Модуль отображения контролируется автоматической системой перезагрузки и отключения (Watchdog). 
Если устройство не имеет имеет команды контролируемого перезапуска, и при его включении произошел сбой, Watchdog автоматически выключит все устройства отображения и выдаст ошибку на консоль.
Если устройство имеет команду контролируемого перезапуска (сейчас это PCWithDaemon, VLCPlayer, VLCPlayer2Screen), то Watchdog работает по следующему принципу:

* sdfsdf
