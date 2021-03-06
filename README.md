## Плебисцит

Скрипт предназначен для выгрузки данных о голосовании (и широкомасштабных фальсификациях на нём) по поправкам в Конституции РФ.

Чтобы его использовать, клонируйте репозиторий и установите зависимости (axios, retry-axios, cheerio и iconv):

```bash
npm install
```

После этого зайдите на [страницу результатов ЦИКа](http://www.vybory.izbirkom.ru/region/region/izbirkom?action=show&root=1&tvd=100100163596969&vrn=100100163596966&region=0&global=1&sub_region=0&prver=0&pronetvd=null&vibid=100100163596969&type=465) в браузере и введите капчу. Откройте консоль разработчика и скопируйте значение куки `izbirkomSession`. Вставьте его в 23 строку скрипта вместо `PASTE_YOUR_SESSIONID_HERE`:

```javascript
...
    headers: {
      Cookie: 'izbirkomSession=PASTE_YOUR_SESSIONID_HERE', // <- Вставить куку после ввода капчи из браузера
    },
...
```

После этого можно запустить скрипт:
```bash
node scrape.js
```

Он отрабатывает достаточно быстро, минут за 15. Если нужно будет запустить сбор данных снова — возможно, придётся обновить капчу (скрипт выкинет ошибку при запуске).

Полученные данные скрипт сохраняет в виде JSON-файлов в папку с именем вида `DD.MM.YYYY H-mm`. Подробно структуру данных можно посмотреть в функции `processLevel`, весь код снабжён подробными комментариями. 

Запросы скрипт делает весьма щадяще, по очереди. Тем не менее, сервера ЦИКа иногда могут сильно задуматься и перестать отвечать (как правило, секунд на 10-20, но может и совсем всё умереть). Тогда нужно процесс перезапустить с начала.