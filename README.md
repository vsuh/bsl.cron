### Внешние регламентные задания
---

Предполагается, что в *ИБ 1С* раз в сутки (например) запускается фоновое задание, которое читает из файла `do.bsl` текст модуля и исполняет его методом `Выполнить()`

например так:

    гуПуть = "\\kopt-app-01\com\bsl.cron";
    гуФайл = Новый Файл(гуПуть+"\do.bsl");
    Если Не гуФайл.Существует() Тогда
      ВызватьИсключение "Critical exception: Не найден "+гуФайл.ПолноеИмя;
    КонецЕсли;

    гуЧтен = Новый ЧтениеТекста(гуФайл.ПолноеИмя, "UTF-8");
    гуПроц = гуЧтен.Прочитать();
    гуЧтен.ЗАкрыть();

    Попытка
      Выполнить(гуПроц);
    Исключение
      Сообщить("Не удалось выполнить; "+ОписаниеОшибки());
    КонецПопытки;


В `do.bsl` анализируются файлы `globals.json` для определения глобальных настроек
и `tasks.json` для выяснения какой метод нужно выполнять сегодня.
Имя методов соответствует `*.bsl` файлам, которые читаются и исполняются в `do.bsl`
Даты исполнения можно задать тремя вариантами:

- **dow** - день недели 1..7
- **dom** - день месяца 1..31
- **date** -  дата в формате ISO `ГГГГ-ММ-ДД`

2016&copy;VSCraft


