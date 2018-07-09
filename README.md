# RabbitMQ PARSEC Server

Этот пакет предназначен для запуска серверов AMQP RabbitMQ
с мандатными уровня под ОС «Astra Linux Special Edition».

Авторы: [Лаборатория 50](http://лаборатория50.рф) team@lab50.net.

## Лицензия

Все материалы распространяются на условиях
стандартной общественной лицензии [GNU (GPL)](http://www.gnu.org/copyleft/gpl.html) версии 3.

Полный текст лицензии находится в файле COPYING.

## В чем проблема, Холмс?

Штатный пакет брокера RabbitMQ в составе дистрибутива 
Astra Linux Special Edition не поддерживает мандатное разграничение
доступа.

По-настоящему это делать долго и не просто. Данная утилита пригодится
для простейшего решения проблемы — запуск нескольких брокеров RabbitMQ
под разными мандатными уровнями. Экземпляры могут работать паралелльно на одном
сервере.

Каждый брокер запускается без привилегии PRIVSOCK, таким образом к нему могут
подключится только клиенты с аналогичными мандатными привилегиями.

## Но как, Холмс?

Настоящий пакет не является заменой штатному rabbitmq-server, а
предоставляет альтернативный демон `rabbitmq-server-parsec`. Все что
он делает — запускает брокер RabbitMQ с необходимым мандатным уровнем.

Пакет совместим со штатным rabbitmq-server. После установки пакета
данные будут доступны у экземпляра с нулевым уровнем.

Алгоритм установки:

 1. Установите пакет rabbitmq-server-parsec.

 2. Задайте пользователю rabbitmq необходимые мандатные уровни, например:

    pdpl-user -m 0:2 rabbitmq

 3. Скопируйте файл `rabbitmq-server-parsec` в каталоге `/etc/init.d` необходимое
    число раз с суффиксом `:Х`, где Х — мандатный уровень.
    Например:

    cp rabbitmq-server-parsec rabbitmq-server-parsec:1

 4. Включите демон в автозагрузку: `update-rc.d rabbitmq-server-parsec:1 default`

Для запуска rabbitmq-server используется утилита
[start-stop-parsec-daemon](https://github.com/laboratory50/start-stop-parsec-daemon).
