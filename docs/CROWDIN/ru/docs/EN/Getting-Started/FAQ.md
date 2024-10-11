# Часто задаваемые вопросы по работе ИПЖ

Хотите добавить вопросы в ЧаВо? Читайте [инструкции](../make-a-PR.md)

# Общие настройки

## Можно ли скачать установочный файл AAPS?

Нет. Для AndroidAPS не предоставляется загружаемый файл apk. Его надо [скомпилировать](../Installing-AndroidAPS/Building-APK.md) самостоятельно. Причина вот в чем:

AndroidAPS создан для управления помпой и подачи инсулина. В соответствии с действующим Европейским законодательством, все системы, классифицируемые как IIa или IIb, являются медицинскими устройствами, подлежащими обязательной сертификации (получение знака CE), что в свою очередь требует соответствующих исследований и одобрений. Распространение несертифицированных устройств незаконно. Аналогичные законы существуют и в других частях мира.

Это положение не ограничивается торговлей, но относится к любому виду распространения (даже безвозмездному). Создание медицинского устройства для себя является единственным вариантом, не затрагиваемым этими правилами.

Именно поэтому распространение в виде готовых приложений (apk) недоступно.

(FAQ-how-to-begin)=

## С чего начать?

Прежде всего, нужно **подготовить компоненты, которые работают с ипж**:

- [Совместимая с AAPS(ИПЖ) инсулиновая помпа](./Pump-Choices.md) 
- смартфон на Андроид (Apple iOS не поддерживается в AndroidAPS -рассмотрите вариант [iOS Loop](https://loopkit.github.io/loopdocs/))
- [Система непрерывного мониторинга глюкозы крови (ГК)](../Configuration/BG-Source.md). 

Во-вторых, нужно **настроить оборудование**. Смотрите [пример установки с пошаговым руководством](Sample-Setup.md).

В-третьих, нужно **настроить компоненты программного обеспечения**: AAPS и источники мониторинга.

В-четвертых, нужно изучить и **понять исходный дизайн OpenAPS для проверки параметров лечения**. Фундаментальный принцип замкнутой петли: скорость подачи базала и соотношение инсулин/углеводы должны быть точно выверены. Все рекомендации, выдаваемые ИПЖ предполагают, что ваши базовые потребности в инсулине удовлетворены и любые пики и провалы характеристики ГК - следствие других факторов, требующих дополнительных настроек ( нагрузка, стресс и пр.). В настройках ИПЖ введены ограничения безопасности (см. максимально допустимый базальный уровень в [Архитектуре OpenAPS](https://openaps.org/reference-design/)), которые означают, что вам не придется тратить допустимые дозировки на исправление неправильной базы. Например, если перед едой вы часто ставите временную цель на пониженный уровень ГК, вам, скорее всего, требуется настройка базы. Вы можете использовать [автонастройку](https://openaps.readthedocs.io/en/latest/docs/Customize-Iterate/autotune.html#phase-c-running-autotune-for-suggested-adjustments-without-an-openaps-rig) для обработки массива ваших данных и определения необходимости коррекции базальной скорости, фактора чувствительности к инсулину ISF и углеводного коэффициента CR. Либо протестировать и определить скорость базала [старым способом](https://integrateddiabetes.com/basal-testing/).

## Важные практические аспекты

### Защита паролем

Если вы считаете необходимым исключить несанкционированное изменение настроек ИПЖ, вы можете закрыть паролем настройку, выбрав в меню "Пароль для настроек" и установив пароль на этот раздел. В следующий раз, когда вы войдете в это меню, система запросит пароль и не позволит поменять его без корректного ввода. Если в дальнейшем вы хотите удалить пароль, зайдите в это меню снова и удалите текст.

### Смарт-часы Android Wear

Если вы планируете использовать Android Wear для изменения болюса или настроек, нужно убедиться, что сообщения от AAPS не блокируются. Подтверждение действий происходит через уведомление.

(FAQ-disconnect-pump)=

### Отключение помпы

Если вы снимаете помпу на время душа, купания, занятия спортом или других действий, то, чтобы активный инсулин IOB правильно отражался в системе, следует проинформировать AAPS, что инсулин не вводится,.

Помпу можно отключить с помощью нажатия на символ цикла на [главном экране AAPS](Screenshots-loop-status).

### Рекомендации основаны не на одном показании мониторинга

Для безопасности, рекомендации системы делаются не на одном показании ГК, а на среднем из последних значений (с учетом скользящей дельты). Поэтому, если пропущено несколько показаний, понадобится время на то, чтобы AAPS снова начал компенсацию ГК в режиме замкнутого цикла.

### Дополнительные ресурсы

Вот несколько блогов с полезными советами, которые помогут понять практику работы ИПЖ:

- [Подробные Настройки ](https://seemycgm.com/2017/10/29/fine-tuning-settings/) (см.мой мониторинг)
- [Почему так важна длительность работы инсулина DIA](https://seemycgm.com/2017/08/09/why-dia-matters/) (см. мой мониторинг)
- [Как ограничить пики после питания](https://diyps.org/2016/07/11/picture-this-how-to-do-eating-soon-mode/) #DIYPS
- [Гормоны и autosens](https://seemycgm.com/2017/06/06/hormones-2/) (см.мой мониторинг)

## Какое запасное оборудование рекомендуется брать с собой?

Прежде всего, вам необходимо иметь стандартный набор для больного диабетом 1го типа. При пользовании AAPS настоятельно рекомендуется также иметь:

- Банк батарей и кабель для зарядки смартфона, часов и (если это необходимо) BT reader или Link device
- Батарейки для помпы
- Действующие дистрибутивы [apk](../Installing-AndroidAPS/Building-APK.md) и [файлы настроек](../Usage/ExportImportSettings.md) для AAPS и других приложений, (напр. xDrip+, BYO Dexcom) как локально, так и в облаке (Dropbox, Google Drive).

## Как надежно и безопасно закрепить сенсор для мониторинга?

Его можно закрепить лейкопластырем. Существует несколько предварительно перфорированных 'накладок' для распространенных систем мониторинга (искать в Google, eBay. Amazon. Озон, Wildberries и т. п.). Некоторые пользователи ИПЖ применяют более дешевые стандартные кинезезиотейпы или лейкопластыри.

Его можно закрепить держателем. В продаже есть держатели CGM/FGM с повязками (поиск Google, eBay Amazon, Озон, Wildberries).

# Настройки AAPS

Этот список поможет оптимизировать настройки. Начните сверху и двигайтесь вниз. Старайтесь выверить одну настройку прежде чем переходить к другой. Двигайтесь небольшими шагами, не вносите большие изменения сразу. Можно использовать автонастройку [Autotune](https://autotuneweb.azurewebsites.net/) которая даст общее направление, но принимайте рекомендации не вслепую и не во всех ситуациях. Обратите внимание, что настройки дополняют друг друга - вы можете иметь "неправильные" настройки, которые в некоторых обстоятельствах работают хорошо (например, если слишком высокий базал установлен одновременно со слишком высоким углеводным коэффициентом CR), но в других не работают. Это означает, что нужно учитывать все настройки и убедиться, что они совместно работают в различных обстоятельствах.

## Продолжительность активности инсулина DIA

### Описание & тестирование

Длительность времени, которое инсулин падает до нуля.

Довольно часто задается слишком коротким. Правильная настройка для большинства - не менее 5 часов, иногда 6 или 7.

(FAQ-impact)=

### Результат

Слишком малое значение продолжительности действия инсулина DIA может привести к низкой ГК. И наоборот.

Если DIA слишком короткий, AAPS слишком рано решит, что предыдущий болюс израсходован, и, при все еще высокой ГК, добавит инсулин. (Фактически, алгоритм не выжидает, а продолжает добавлять инсулин, предсказывая, что произойдет). Это, по сути, создает «затоваривание инсулина», о котором AAPS не знает.

Пример слишком короткого DIA - это высокая ГК, за которой следует избыточная коррекция и в результате - низкая ГК.

## График базала (ед/ч)

### Описание & тестирование

Количество инсулина в заданном часовом блоке для поддержания ГК на стабильном уровне.

Проверьте настройки базы, приостановив цикл и пропуская приемы пищи, чтобы контролировать изменения ГК в течение 5 часов после еды. Повторите несколько раз.

Если ГК падает, то уровень базы слишком велик. И наоборот.

### Результат

Повышенная скорость подачи базала может привести к низкому уровню ГК. И наоборот.

AAPS по умолчанию строит свой алгоритм отталкиваясь от скорости базала. Если базал слишком высокий, то «нулевая временная скорость» будет определяться при большем отрицательном значении активного инсулина IOB, чем нужно. Это приведет к тому, что AAPS будет давать больше коррекций, чем следует, чтобы привести активный инсулин IOB к нулю.

Таким образом, слишком высокая скорость подачи базала создаст низкую ГК как на стандартной базе, так и несколько часов спустя, так как AAPS еще корректирует к цели.

И наоборот, слишком низкая скорость базы может привести к высоким ГК, и невозможности снизить уровень до целевого.

## Фактор чувствительности к инсулину (ISF) (ммоль/л/ед или мг/дл/ед)

### Описание & тестирование

Ожидаемое снижение ГК после подачи 1ед. инсулина.

Если база рассчитана верно, вы можете определить ISF, приостановив цикл и убедившись что IOB равен нулю, принять несколько таблеток глюкозы, чтобы получить стабильно «высокую» гликемию.

Затем введите предполагаемое количество инсулина (по текущему соотношению 1/ISF), чтобы прийти к целевой ГК.

Будьте осторожны, так как довольно часто величина слишком занижена. Слишком низкая ГК означает, что 1 ед опустит ГК быстрее, чем ожидалось.

### Результат

**Более низкий ISF** (например, 40 вместо 50) означает, что каждая единица инсулина меньше опускает ГК. Это приводит к более агрессивной / сильной коррекции со стороны алгоритма с **бОльшим количеством инсулина**. Если она слишком мала, можно получить низкий уровень ГК.

**Более высокий ISF** (например, 45 вместо 35) означает, что каждая единица инсулина более значительно опускает ГК. Это приводит к менее агрессивной коррекции со стороны алгоритма с **меньшим количеством инсулина**. Если ISF слишком велик, можно получить высокий уровень ГК.

**Пример:**

- ГК - 190 мг/дл (10,5 ммол/л), а цель - 100 мг/дл (5,6 ммол/л). 
- Нужно скорректировать 90 мг/дл (= 190 - 100).
- Чувствитльность ISF = 30 -> 90 / 30 = 3 единицы инсулина
- Чувствитльность ISF = 45 -> 90 / 45 = 2 единицы инсулина

Слишком низкая заданная чувствительность (бывает нередко) может привести к "чрезмерной коррекции" ГК, поскольку AAPS будет считать, что ему нужно больше инсулина для корректировки высокой гликемии, чем на самом деле. Это может привести к «американским горкам» (особенно при голодании). В этом случае нужно увеличить ISF. Тогда ААПС уменьшит корректирующие дозы, что позволит избежать чрезмерной коррекции, которая приводит к низкой ГК.

В то же время, задание слишком высокой чувствительности ISF может привести к недостаточной коррекции, когда ГК останется выше цели, это особенно заметно за ночь.

## Углеводный коэффициент (IC) (г/ед)

### Описание & тестирование

Углеводы в граммах на каждую единицу инсулина.

Некоторые пользователи также используют I:C в качестве сокращения вместо IC или говорят о коэффициенте углеводов (carb ratio, CR).

Полагая, что база задана верно, можно проверить этот коэффициент, убедившись в отсутствии активного инсулина IOB при нормальной гликемии. Для этого надо съесть известную пищу с известным количеством углеводов и подав на нее расчетное количество инсулина, основываясь на текущем соотношении IC. Лучше всего есть пищу, которую вы обычно едите в это время дня и точно посчитать ее углеводы.

> **ПРИМЕЧАНИЕ:**
> 
> В некоторых европейских странах для определения количества инсулина, необходимого для компенсации пищи, использовались т. н. хлебные единицы. В начале 1 хлебная единица равнялась 12г углеводов, позже некоторые стали считать ее как 10 г.
> 
> В такой модели подсчетов количество углеводов было фиксированной величиной, а количество инсулина-переменной. ("Сколько инсулина нужно для того, чтобы компенсировать одну хлебную единицу?")
> 
> При использовании коэффициента инсулин-углеводы IC количество инсулина фиксируется, а количество углеводов становится величиной переменной. ("Сколько грамм углеводов может компенсироваться одной единицей инсулина?")
> 
> Пример:
> 
> Множитель хлебной единицы (ХЕ = 12г. углеводов): 2,4 ед./ХЕ -> Требуется 2,4 ед. инсулина для компенсации одной ХЕ.
> 
> Соответствующее соотношение инсулин-углеводы IC: 12г /2,4 ед -> 5,0 г. углеводов можно компенсировать одной единицей инсулина.
> 
> Множитель ХЕ 2,4 ед/12g ===> IC = 12 г/2,4 ед = 5,0 г/ед
> 
> Таблицы преобразования доступны в Интернете: [ здесь ](https://www.mylife-diabetescare.com/files/media/03_Documents/11_Software/FAS/SOF_FAS_App_KI-Verha%CC%88ltnis_MSTR-DE-AT-CH.pdf).

### Результат

**Более низкий углеводный коэффициент IC** = меньше еды на единицу инсулина, то есть вы получаете больше инсулина на фиксированное количество углеводов. Может также назваться «более агрессивным».

**Более высокий углеводный коэффициент IC** = больше еды на единицу инсулина, то есть вы получаете меньше инсулина на фиксированное количество углеводов. Может также назваться «менее агрессивным».

Если после усваивания пищи и возвращения активного инсулина IOB к нулю, ГК остается выше чем до еды, высока вероятность того, что IC слишком велик. И наоборот, если ГК ниже, чем перед едой, IC слишком мал.

# Алгоритм APS

## Почему на вкладке "помощник болюса OPENAPS AMA" время действия инсулина DIA показано как 3, хотя у меня в профиле другая величина?

![Мастер болюса AMA 3 часа](../images/Screenshot_AMA3h.png)

В помощнике болюса AMA "DIA" не означает "длительность действия инсулина". Этот параметр раньше привязывался к длительности действия инсулина DIA. Теперь же он означает, 'время, за которое нужно завершить коррекцию'. Он не имеет ничего общего с расчетом активного инсулина IOB. В алгоритме OpenAPS SMB больше нет необходимости в этом параметре.

## Профиль

### Почему следует устанавливать минимум продолжительности действия инсулина DIA на 5 часов вместо 2-3 часов?

Хорошо объяснено в [этой статье](https://www.diabettech.com/insulin/why-we-are-regularly-wrong-in-the-duration-of-insulin-action-dia-times-we-use-and-why-it-matters/). Не забудьте `АКТИВИРОВАТЬ ПРОФИЛЬ` после изменения продолжительности действия инсулина DIA.

### Что заставляет алгоритм цикла часто понижать мою ГК до гипогликемических значений в отсутствии углеводов COB в организме?

В первую очередь, проверьте значения скорости подачи базала и проверьте работу базы безуглеводным test'ом. Если все верно, то такое поведение обычно вызвано слишком низким значением чувствительности к инсулину ISF. Слишком низкая чувствительность ISF обычно выглядит так.

![Чувствительность ISF слишком низкая](../images/isf.jpg)

### Что вызывает высокие постпрандиальные пики в замкнутом цикле?

В первую очередь, проверьте значения скорости подачи базала и проверьте работу базы безуглеводным test'ом. Если все правильно, и гликемия падает до целевого значения после полного усвоения углеводов, попробуйте за некоторое время до еды установить временную цель "ожидаемый прием пищи" в AAPS или продумайте подходящее время преболюса с эндокринологом. Если гликемия слишком высока после еды и после полного усваивания углеводов, подумайте о снижении углеводного коэффициента ( соотношения инсулин/углеводы) IC с эндокринологом. Если гликемия слишком высока во время усвоении углеводов COB и слишком низка после их полного усвоения, подумайте об увеличении углеводного коэффициента IC и о надлежащем времени преболюса с эндокринологом.

# Другие настройки

## Настройки Nightscout

### Клиент NScout AndroidAPS выдает ошибку 'не разрешено' и не передает данные. Что делать?

В клиенте NSClient проверьте 'Настройки подключения'. Возможно, вы в закрытой для вас зоне WLAN или активировали опцию "подключаться только при зарядке", а ваш кабель зарядки не подключен.

## Настройки мониторинга

### Почему AAPS выдает сообщение: 'Источник ГК не поддерживает расширенную фильтрацию'?

Если у вас иной источник данных ГК чем Dexcom G5 или G6 в нативном режиме xDrip, вы получите это уведомление на вкладке OpenAPS. Для более подробной информации см [Сглаживание данных ГК](../Usage/Smoothing-Blood-Glucose-Data-in-xDrip.md).

## Помпа

### Где поместить помпу?

Есть многочисленные возможности размещения помпы. При этом неважно, пользуетесь вы приложениями ИПЖ или нет.

### Батареи

Работа в замкнутом цикле быстрее расходует батарею поскольку система больше взаимодействует по блутусу чем обычная помпа в ручном режиме. Лучше менять батарею при 25% так как после этого связь становится труднее. Можно настроить предупредительные сигналы батареи помпы через переменную PUMP_WARN_BATT_P на сайте Nightscout. Способы увеличить срок жизни батареи включают:

- уменьшить длительность работы экрана LCD (в меню настроек помпы)
- уменьшить длительность работы подсветки (в меню настроек помпы)
- выбрать уведомления в виде звукового сигнала а не вибрации (в меню настроек помпы)
- Нажимать кнопки помпы только для перезагрузки; для просмотра журналов, уровня батареи и объема резервуара помпы пользоваться смартфоном с AAPS.
- На некоторых телефонах AAPS периодически закрывается для экономии энергии или высвобождения оперативной памяти. Когда AAPS вновь инициализируется, то при каждом старте устанавливает соединение Bluetooth с помпой и перечитывает текущую базальную скорость и журнал болюсов. Это расходует батарею. Чтобы увидеть, происходит ли это, перейдите в Настройки > NSClient и включите 'Отправлять запись о запуске приложения в NS'. Nightscout будет получать данные о событии при каждом перезапуске AAPS, что облегчит отслеживание проблемы. Чтобы уменьшить расход батареи при таких событиях, включите AAPS в список разрешенных приложений в настройках батареи телефона и тогда монитор расхода энергии перестанет выключать AAPS.
    
    Например, внести в белый список на смартфоне Samsung Android Pie:
    
    - Перейти к параметрам-> Уход за устройством-> Аккумулятор 
    - Прокрутите страницу пока не найдете AndroidAPS и выберите его
    - Снимите флажок с "Переводить приложение в спящий режим
    - Также зайдите в Настройки -> Приложения -> (символ три круга в правой верхней части экрана) выбираем "особый доступ" -> оптимизировать использование батареи
    - Прокрутите страницу до AndroidAPS и убедитесь, что галочка выбора снята.

- протирайте контакты батареи спиртовыми салфетками, чтобы на ней не оставались следы заводской смазки.

- в [помпах DanaR/RS ](../Configuration/DanaRS-Insulin-Pump.md)процедура запуска батареи подает импульс высокого напряжения для устранения заводской пленки (которая предотвращает потерю энергии при хранении), но это не всегда срабатывает на 100%. Либо удалите и заново вставьте батарею 2-3 раза до тех пор, пока на экране помпы заряд батареи не покажет 100%, либо замкните контакты батареи на долю секунды при помощи ключа батареи, чтобы удалить этот налет.
- см. также советы для [конкретных типов батареи](Accu-Chek-Combo-Tips-for-Basic-usage-battery-type-and-causes-of-short-battery-life)

### Замена картриджей и катетеров

Замена картриджей не может осуществляться через AAPS, ее следует как и раньше, делать непосредственно на помпе.

- Нажмите и удерживайте кнопку "Открытый цикл"/"Замкнутый цикл" на вкладке "Главный экран" AAAPS и выберите "Приостановка цикла на 1ч.'
- Отключите помпу и замените резервуар в соответствии с инструкцией помпы.
- Заполнить инфузионный набор можно и непосредственно с помпы. В этом случае пользуйтесь кнопкой [первичное заполнение инфузионного набора](CPbefore26-pump) во вкладке "Действия" только для внесения записи изменений.
- После переподключения помпы запустите цикл долгим нажатием на 'Приостановлено (X мин.)'.

Однако замена катетера происходит не через функцию "первичного заполнения инфузионного набора" на помпе, но заполняет катетер с помощью болюса, который не отражается в истории болюса. Это означает, что текущая временная скорость базала не прерывается. На вкладке Действия при помощи кнопки [ЗАПОЛНИТЬ](CPbefore26-pump) задайте то количество инсулина, которое необходимого для заполнения инфузионного набора и начните первичное заполнение. Если этого количества недостаточно, повторите заполнение. Вы можете установить кнопки по умолчанию в Настройках > Другое > Заполнить/Инициировать стандартные количества инсулина. В инструкции к инфузионному набору вы найдете объемы единиц для первичного заполнения в зависимости от длины иглы и длины трубки.

## Фоновый рисунок

Обои AAPS можно найти для телефона на странице [ Телефоны ](Phones-phone-background).

## Повседневное применение

### Гигиена

#### Что делать при приеме душа или ванной?

Помпу можно снять при приеме душа или ванной. За этот короткий промежуток времени вам может не понадобиться, но вы должны сообщить AAPS о том, что отключились для того, чтобы вычисления IOB были правильными. См. [ описание выше ](FAQ-disconnect-pump).

### На работе

В зависимости от вида работы, возможно, вы используете иные методы терапии в рабочие дни. В этом случае есть смысл рассмотреть переключение [профиля](../Usage/Profiles.md) на типичный рабочий день. Например, профиль больше 100% можно установить, если работа не очень тяжелая (напр. сидя за столом), или меньше 100%, если вы активны и на ногах весь день. Также можно рассмотреть возможность установки высокой или низкой временной цели или [смены профиля](Profiles-time-shift), если начинаете намного раньше или позже обычного, или в разные смены. Также можно создать второй профиль (например, дом' и 'работа') и при необходимости переключаться с профиля на профиль.

## Отдых

(FAQ-sports)=

### Спорт

Пересмотрите свои старые спортивные привычки доаппсовских времен. Если вы как и прежде съедаете один или несколько углеводов на спорт, теперь система замкнутого цикла распознает их и соответствующим образом скорректирует.

Итак, в организме будет больше углеводов, но в то же время петля будет противодействовать и подавать инсулин.

При работе с алгоритмом ИПЖ следует выполнить следующие действия:

- Уменьшите [ профиль ](../Usage/Profiles.md) < 100%.
- Установите [временную цель нагрузка](temptarget-activity-temp-target) выше стандартного целевого значения.
- Если вы пользуетесь микроболюсами SMB, убедитесь, что [ Включить SMB с высокими временными целями "](Open-APS-features-enable-smb-with-high-temp-targets) и [" Включить SMB всегда " ](Open-APS-features#enable-smb-always) неактивны.

Важное значение имеет предварительная и последующая обработка этих настроек. Внесите изменения до занятий спортом и учитывайте эффект наполнения мышц.

Если вы занимаетесь спортом регулярно в одно и то же время (например, спортивные занятия в тренажерном зале), можно пользоваться [ автоматизацией automation ](../Usage/Automation.md) для изменения профиля и временных целей TT. Автоматизация на основе геолокации также неплохая идея, но делает предварительную обработку более сложной.

Процент изменения профиля, величина временной цели при нагрузках и наилучшее время для внесения изменений индивидуальны. Начните с более безопасных параметров (например с низким процентоом профиля и более высокими временными целями).

### Секс

Можете снять помпу для "свободы" но следует проинформировать об этом AAPS, чтобы расчеты активного инсулина IOB были правильными. См. [ описание выше ](FAQ-disconnect-pump).

### Употребление алкоголя

Употребление алкоголя является рискованным в режиме замкнутого цикла, так как алгоритм ИПЖ не может правильно предсказать влияние алкоголя на ГК. Следует выработать свой собственный метод подхода к этому вопросу с помощью следующих функций в AndroidAPS:

- Деактивировать режим замкнутого цикла и разбираться с диабетом вручную или
- устанавливать высокие временные цели и отключать незапланированный прием пищи UAM, чтобы избежать увеличения активного инсулина IOB из-за непредусмотренной еды или
- переключить профиль на величину, заметно менее 100% 

При употреблении алкоголя всегда нужно следить за мониторингом, чтобы вручную избежать гипокликемии, съедая углеводы.

### Сон

#### Как обеспечить работу цикла ночью без воздействия мобильного и WIFI излучения?

Многие пользователи ночью переводят телефон в режим авиаперелета. Если вы хотите, чтобы AAPS поддерживал вас во время сна, действуйте следующим образом (будет работать только с локальным источником ГК, таким как xDrip+ или ['самостоятельно построенным приложением Dexcom BYODA'](DexcomG6-if-using-g6-with-build-your-own-dexcom-app), но НЕ будет работать, если данные ГК поступают с сайта Nightscout):

1. Включите режим авиаперелета на вашем мобильном устройстве.
2. Подождите, пока режим авиаперелета не будет активирован.
3. Включите Блутус.

Теперь вы не принимаете звонки и не подключены к Интернету. Но цикл продолжает работу.

У некоторых пользователей обнаружились проблемы с локальной трансляцией (AAPS не получает данные от xDrip+) в режиме авиаперелета. Перейдите в Настройки xdrip+ > Inter-app settings > Identify receiver и введите `info.nightscout.androidaps`.

![xDrip+ Inter-app Settings Identify receiver](../images/xDrip_InterApp_NS.png)

### При переездах

#### Как справляться с изменениями часового пояса?

Если у вас DanaR или DanaR для корейского рынка делать ничего не надо. С другими помпами смотрите страницу [Пересечение часовых поясов с помпами](../Usage/Timezone-traveling.md) для более подробной информации.

## Медицинские аспекты

### Госпитализация

Если вы хотите поделиться информацией об AndroidAPS и ИПЖ с врачами, можете распечатать [руководство по AAPS для медработников](../Resources/clinician-guide-to-AndroidAPS.md).

### На приеме у эндокринолога

#### Отчетность

Вы можете либо распечатать отчеты вашего сайта Nightscout (https://вашсайт.com/report) или перейти в [Nightscout Reporter](https://nightscout-reporter.zreptil.de/).

# Частые вопросы из Discord и ответы на них...

## Моя проблема здесь не указана.

[Информация о получении помощи.](Connect-with-other-users-i-m-getting-stuck-what-do-i-do-who-can-i-ask)

## Моя проблема не перечислена здесь, но я нашел решение

[Информация о получении помощи.](Connect-with-other-users-i-m-getting-stuck-what-do-i-do-who-can-i-ask)

**Напомните нам о добавлении вашего решения в этот список!**

## AAPS останавливается каждый день в одно время.

Остановите защиту Google Play Protect. Проверьте "очистку" приложений (при помощи CCleaner и т. д.) и удалите их. AAPS / 3 точки меню / О программе / Перейдите по ссылке "Работать в фоновом режиме" и остановите все оптимизации батареи.

## Как организовать резервные копии ?

Регулярно экспортируйте настройки: после замены каждого пода, после изменения профиля, когда подтверждено прохождение цели, если заменили помпу… Даже если ничего не изменится, экспортируйте настройки раз в месяц. Сохраняйте несколько старых файлов экспорта.

Скопируйте на интернет-диске (Dropbox, Google etc) : все приложения, которые использовали для установки приложений на телефон (AAPS, xDrip, BYODA, Patched LibreLink…), а также экспортированные настройки из всех этих приложений.

## Проблемы, ошибки при создании приложения.

Пожалуйста,

- проверьте [Устранение неполадок Android Studio](troubleshooting_androidstudio-troubleshooting-android-studio) на наличие типичных ошибок и
- [пошаговые инструкции](https://docs.google.com/document/d/1oc7aG0qrIMvK57unMqPEOoLt-J8UT1mxTKdTAxm8-po).

## Я застрял на цели и нуждаюсь в помощи.

Сохраните экран вопросов и ответов. Отправьте сообщение на канал Discord AAPS. Не забудьте указать какие опции вы выбираете (или нет) и почему. Вы получите подсказки и помощь, но ответы надо найти самостоятельно.

## Как сбросить пароль в AAPS v2.8.x ?

Откройте меню hamburger, запустите мастер настройки и введите новый пароль по запросу. Вы можете выйти из мастера после установки нового пароля.

## Как сбросить пароль в AAPS v3.x

Документация [здесь](update3_0-reset-master-password).

## Мой Link/помпа/pod не отвечает (RL/OL/EmaLink…)

На некоторых телефонах Bluetooth разъединяется с Link (RL/OL/EmaL...).

Некоторые также отмечают плохую коммуникацию с RileyLink (AAPS сообщает, что они подключены, но не выполняют команды)

Самый простой способ заставить эти части работать - : 1/ Стереть Link из AAPS 2 / Отключить питание Link 3/выйти из AAPS через 3-точечное меню программы 4/ Долгое нажатие на значок AAPS, меню Android, информация о приложении AAPS, Принудительно остановить AAPS и затем очистить память кэша (Не удаляйте основную память !) 4bis/ Некоторым телефонам после этого может понадобиться перезагрузка здесь. Можно попробовать без перезагрузки. 5/ Включить Link 6/Запустить AAPS 7/Вкладка Pod, меню 3 точки, поиск и подключение (Riley)Link

## Ошибка сборки: слишком длинное имя файла

Во время сборки программы получаю ошибку, что имя файла слишком длинное. Возможные решения: Переместите источники в директорию ближе к корневой директории диска (например "c:\src\AndroidAPS-EROS").

Из Android Studio: Убедитесь, что "Gradle" завершил синхронизацию и индексирование после открытия проекта с GitHub. Выполните Build->Clean Project прежде чем выполнять Rebuild Project. Выполните команду File>Инвалидация кэша и перезагрузите Android Studio.

## Внимание: Выполняется версия разработчика (dev). Замкнутый цикл отключен

AAPS не работает в "режиме разработчика". AAPS показывает сообщение: "запуск dev версии. Замкнутый цикл отключен".

Убедитесь, что AAPS запущен в "режиме разработчика": Поместите файл с именем "engineering_mode" в папку "AAPS/extra". Любой файл подойдет если он назван правильно. Обязательно перезапустите AAPS, чтобы найти файл и перейти в "режим разработчика".

Совет: сделайте копию существующего лог-файла и переименуйте его в "engineering_mode" (примечание: без расширения!).

## Где найти файлы настроек?

Файлы настроек будут храниться во внутреннем хранилище телефона в каталоге "/AAPS/preferences". ВНИМАНИЕ: Убедитесь что помните пароль, без него вы не сможете импортировать зашифрованный файл настроек!

## Как настроить экономию батареи?

Правильная настройка управления питанием важна для предотвращения остановки AAPS и связанных с ним приложений и служб, когда телефон не используется. Если не сделать этого, AAPS не сможет выполнить соединения по Bluetooth с сенсором и Rileylink (RL) может быть выключен, что приведет к "отключению "помпы" оповещений "помпа недоступна" и ошибкам связи. На телефоне перейдите в Настройки->Приложения и отключите экономию заряда батареи для AAPS xDrip или самостоятельно собранное приложение BYODA/Dexcom Системное приложение Bluetooth (возможно, необходимо сначала выбрать системные приложения) Или полностью отключить все энергосбережения на телефоне. В результате батарея может разряжаться быстрее, но вы смжете выяснить, не вызывается ли проблема экономией заряда аккумулятора. Способ экономии аккумулятора в значительной степени зависит от марки телефона, модели и/или версии операционной системы. Из-за этого почти невозможно дать инструкции по правильной настройке экономии заряда аккумулятора. Экспериментируйте, чтобы выяснить, какие настройки лучше для вас. Дополнительную информацию см. также в "Не закрывать приложение"

## Оповещения о недоступности помпы появляются несколько раз в день или ночью.

Ваш телефон может останавливать службы AAPS или даже Bluetooth, что приведет к потере соединения с RL (см. экономия батареи) Настройте оповещения о недоступносте на 120 минут, перейдя в верхнее правое меню, выберите Настройки->Локальные оповещения>Порог недоступности помпы[min].

## Где можно удалить записи терапии в AAPS v3 ?

3 точки меню, выберите терапия, затем меню настроек, и получите различные возможные варианты.

## Настройка приложения AAPSClient remote (для дистанционного управления)

AAPS можно удаленно контролировать через приложение AAPSClient и, при необходимости, через связанное с ним приложение Wear, работающее на часах Android Wear. Обратите внимание, что приложение AAPSClient (дистанционное) отличается от конфигурации NSClient в AAPS, а приложение AAPSClient (дистанционное) Wear отличается от приложения AAPS Wear - для ясности приложения дистанционного управления будут называться 'AAPSClient дистанционный' и 'AAPS Wear дистанционный'.

Чтобы включить дистанционный функционал на AAPSClient дистанционном надо: 1) Установите приложение AAPSClient дистанционный (версия должна совпадать с используемой версией AAPS) 2) Запустите приложение AAPSClient и в мастере настройкипредоставить необходимые разрешения и настройки доступа к вашему сайту Nightscout. 3) На данном этапе вы можете отключить некоторые оповещения, и/или дополнительные настройки, которые регистрируют запуск дистанционного приложения AAPSClient на вашем сайте Nightscout. После этого, дистанционный AAPSClient загрузит данные профиля с сайта Nightscout, на вкладке «Обзор» будут отображаться данные CGM и некоторые данные AAPS, но может не отображать графические данные и указывать, что профиль еще не установлен. 4) Для активизации профиля:

- Включите дистанционную синхронизацию профиля в AAPS > NSClient > Опции
- Активируйте профиль в NSClient дистанционный > Профиль После этого, профиль будет установлен, и AAPSClient дистанционный должен отображать все данные из AAPS. Подсказка: Если график долго отсутствует, попробуйте изменить настройки готображения графика для вызова обновления. 5) Для включения дистанционного управления AAPSClient, выборочно включите настройки AAPS (изменения профиля, Временные Цели, Углеводы и т. д. которыми хотите дистанционно управлять с помощью AAPS > NSClient > Опции . После внесения этих изменений вы сможете дистанционно управлять AAPS с помощью Nightscout или AAPSClient дистанционного.

Если вы хотите контролировать AAPS с помощью приложения AAPSClient Wear дистанционнго, вам понадобится установить AAPSClient дистанционный и соответствующее приложение Wear. Чтобы скомпилировать дистанционное приложение AAPSClient Wear, следуйте стандартным инструкциям по установке/настройке приложения AAPS wear, но при компиляции, выберите вариант AAPSClient.

## У меня горит красный треугольник / AAPS не включает замкнутый цикл/ цикл остановился на низких (LGS) / у меня горит желтый треугольник

Красный и желтый треугольники являются функцией безопасности в AAPS v3.

Красный треугольник означает, что у вас есть дубликаты данных ГК, и AAPS не может точно рассчитать приращение (дельту). При этом невозможно замкнуть цикл. Чтобы очистить красный треугольник, нужно удалить каждое дублируемое значение ГК. Перейдите на вкладку BYODA или xDRIP, долгое нажатие на одну линию, которую вы хотите удалить, пометьте одну из строк, которые задвоены (или через 3 точки меню и Delete, в зависимости от вашей версии AAPS). Если слишком много двойных ГК потребуется сбросить базы данных AAPS,. При этом вы также потеряете статистику, IOB, COB, выбранный профиль.

Возможные причины проблемы: xDrip и/или NS достраивают пропущенные ГК.

Желтый треугольник означает нестабильную задержку между показаниями ГК. Вы не получаете данные каждые 5 минут или ГК отсутствуют. Это зачастую проблема Libre. Это также происходит, когда вы меняете трансмиттер G6. Если желтый треугольник связан с заменой трансмиттера G6, он уйдет сам по себе через несколько часов (около 24 часов). Если у вас Libre, желтый треугольник останется. Цикл может быть замкнут и будет работать правильно.

## Можно ли перенести активный DASH на другое оборудование?

Это возможно. Обратите внимание, что такие действия "не поддерживаются" и "не протестированы", поэтому существует некоторый риск. Лучше всего попробовать процедуру, когда Pod заканчивает срок, так что если что-то пойдёт не так, потери будут невелики.

Критично то, что "состояние" помпы (которое включает в себя MAC-адрес) в AAPS и DASH совпадали при переподключении

## Процедура, которую я применяю:

1) Приостанавливаю DASH. Это гарантирует, что при потере соединения DASH не будет запущенных или отложенных команд, 2) Перевожу телефон в режим авиаперелета чтобы отключились BT ( а также WiFi и мобильные данные). Таким образом, гарантируется AAPS и DASH не будут обмениваться данными. 3) Выполняю экспорт настроек (включая состояние DASH) 4) Копирую файл настроек, только что экспортированный с телефона (так как он находится в режиме полета, который мы не хотим изменять, проще всего использовать USB-кабель) 5) Копирую файл настроек на другой телефон. 6) Импортирую настройки на другой телефон с AAPS. 7) Проверяю вкладку DASH, чтобы убедиться, что смартфон видит Pod. 8) Возобновляю работу Pod. 9) Проверяю вкладку DASH и убеждаюсь, что работает связь с Pod (использую кнопку обновления)

Поздравляем! Все получилось!

*Минутку!!* Ваш прежний телефон все еще считает, что он может подсоединиться к тому же DASH:

1) На главном телефоне выберите "деактивировать". Это безопасно, потому что телефон не имеет способа связи с DASH для фактического отключения Pod (он все еще находится режиме полета) 2. Деактивация приведет к ошибке связи - это ожидаемо. 3) Просто нажмите "повторить" пару раз до тех пор, пока AAPS не предложит опцию "Завершить пользование" POD'ом.

По завершении убедитесь, что AAPS на прежнем телефоне сообщает, что «Нет активного Pod». Теперь вы можете безопасно отключить режим полета.

## Как импортировать настройки из предыдущих версий AAPS в AAPS v3 ?

Можно импортировать только те настройки, которые были экспортированы с помощью AAPS v2.8 или v3.x), Если у вас версия AAPS старше v2,8x, или вам нужны настройки версии старше 2.8x,то сначала придется установить версию v2.8. Импортирeqnt старые настройки v2.x в v2.8. После проверки,что все в порядке, можно экспортировать настройки из v2.8 Установите AAPS v3 и импортируйте настройки v2.8 в v3.

Если вы используете один и тот же ключ для сборки v2.8 и v3, вам не придется импортировать настройки. Можно установить v3 поверх версии 2.8.

Появились новые цели. Нужно будет их подтвердить.