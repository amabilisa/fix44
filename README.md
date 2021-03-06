# fix44
test work

Написать парсер протокола FIX 4.4, который читает сообщения от биржи из текстового файла, строит биржевой стакан (OrderBook) и выводит состояние стакана после каждого обработанного сообщения.

В первом сообщении от биржи транслируется полный биржевой стакан (это т.н. snapshot), далее передаются инкрементальные обновления, каждое обновление добавляет/меняет/удаляет один из ценовых уровней в стакане.

FIX протокол - это текстовый протокол, где все поля представляют собой конструкцию вида <tag>=<value>, разделителем полей является 0x01, в файле с примером данных в качестве разделителя используется символ '^'.
У каждого сообщения есть стандартный заголовок где содержится версия протокола, тип сообщения, длина и т.п. и концовка с контрольной суммой.
В сообщении снапшота есть так называемая группа полей (массив), в этом случае поля входящие в группу повторяются N раз, кол-во повторений задаётся в начале группы.
Базовое описание FIX протокола:
https://en.wikipedia.org/wiki/Financial_Information_eXchange
Описание схем сообщений снапшота: 
https://docs.deribit.com/v2/#market-data-snapshot-full-refresh-w, 
инкрементов: 
https://docs.deribit.com/v2/#market-data-incremental-refresh-x
пример группы полей:
https://ref.onixs.biz/cpp-fix-engine-guide/group__fix-protocol-repeating-group.html
описание что такое стакан (OrderBook):
https://fxssi.com/stock-exchange-order-book-exist-forex-market

Состояние стакана можно печатать в таком виде (можно ограничить вывод до 3-5 первых уровней по каждой стороне):
Total SELL: 3
[2]: price: 20 (1)
[1]: price: 19 (1)
[0]: price: 18 (1)
==================
[0]: price: 12 (1)
[1]: price: 11 (1)
[2]: price: 10 (1)
Total BUY: 3
Total - кол-во уровней на стороне.
В квадратных скобках - номер уровня, нумерация от лучшего по каждой стороне.
После price идёт цена, за ценой в скобках кол-во на уровне

Как результат - главное написать парсинг сообщений и построение стакана, полное соответствие стандарту не требудется, все сообщения имеют корректную длину и контрольную сумму, пробевка этих полей опциональна.
