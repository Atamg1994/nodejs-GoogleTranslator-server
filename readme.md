# Node translator server for XUnity.AutoTranslator
Это скрипт node js сервер предназначер для правельного перевода 
с игнорированием html разметки и числовых значений а также ZM?Z формата числовых значений XUnity.AutoTranslator
Он предназначен для использования с настраиваемыми подключаемыми модулями перевода, назначаемыми конечной точкой, такими как AutoTranslator .
[AutoTranslator](https://github.com/bbepis/XUnity.AutoTranslator)
# Проднобно проблемы 
* у меня были проблемы с тем что при переводе гугл переводчик мог как либо изменить html розметки (добавить пробелы и тдп)
* и проблема с ZM?Z - AutoTranslator таким образом помечат числовые значения чтобы потом после перевода закешыровать их с правельным ключем во измежания повторного перевода
* но! гугл переводчик и ту мог все поламать
* AutoTranslator жыдает обратно свой ZM?Z чтобы подставить какое либо число но гугл перевел ето на мой язык и AutoTranslator не смог найти потому мето примеру 
* "damage done ZMAZ" я вижу "нанесено урона змаз"
* Также у меня вкючена опцыя 
* TemplateAllNumberAway=True
* при переводе она создает темплейты 
* Costs {{A}} month=Стоимость {{A}} месяц
* где {{A}} любое цыфровое значение  что помогает сократить запросы зачем переводит заново если можна подставить 
* а иногда и вовсе AutoTranslator почемуто не верно кешырует значения и потому может вохникнуть дубли перевода с отрилием лис в числеах даже с включеным TemplateAllNumberAway=True
* а еще строка снизу может иметь ети параметры в произвольном порядке (ето как зачарования предемета в каком прядке наложыш в таком и они идут что детает кучю таких строк в кеше AutoTranslator)
* 攻击力 +70\n获得稀有物品几率 +5%\n暴击倍率 +2%\n武技/灵技弹道数量增加 1\n武技/灵技射程增加 16%\n武技/灵技伤害增加 16%\n技能CD减少 1%\n<sprite\=2><color\=#444444>空白属性 可融合</color>
# Кратро что решает мой скрипт 
* Розбивает весь входящий текст через \n таким образом пререводит ети части отдельно и кешытует отдельно
* Фильтрует текст на наличине html розметки , числовых значений , ZM?Z превращает их в тото подобное етому
* (<color\=#00ff00>710</color>/6871) HP  такая строка будет станет такой (<1><2><3>/<4>) HP что посволит после сохранения в кеш и при получении на перевод последующих подобных текстов не переводит их а загрузить с кеша и подставить значения
* грубо говоря делает тоже самое что и задумка TemplateAllNumberAway=True в AutoTranslator но учитывает html розметки
* также ето помогает передотвратить изменение html розметки гугл переводчиком
# установка и запуск
* устанавливаем nodejs
* розархивируем и запускаем start.cmd иди 
* командой node start.js
* AutoTranslator конфиге устанавливаем
* Endpoint=CustomTranslateBanch
* TemplateAllNumberAway=True // не обизатьельно так как мой делает тоже самое но желательно ето еще сократит количестов запросов 
* [CustomTranslateBanch]  находим и пишем такой адрес
* Url=http://localhost:3000/translate_batch_app
* ну и есествено нужно закинуть сам CustomTranslateBanch.dll в папку переводчика
* для MelonLoader game_path\UserLibs\Translators\CustomTranslateBanch.dll
* для Bepinex game_path\BepInEx\plugins\XUnity.AutoTranslator\Translators\CustomTranslateBanch.dll
* мой код использует бесплатный вариант Google Api потому может быть не стабильным
