## 0. Описание документа

Божественные функции - это те функции, которые я как монтажер решаю во время монтажа. Я хочу чтобы код в конечном итоге выполнял действия ниже: 
1. Из целостного клипа вырезал моменты указанные в ТЗ, при этом никак не изменив начальный=целостный клип. (*Это нужно будет чтобы в дальнейшем сделать тизер)
2. Из целостного клипа вырезал моменты указанные в ТЗ.
3. Создал слайды с текстом, написанным в ТЗ.
Есть и еще функции, но их пока не будем автоматизировать. 

Нужна начинать выполнении божественных функций с №1, если алгоритм выполнения божественной задачи №1 использует удаление всех моментов неуказанных в ТЗ и при этом есть только одна секвенция с целостным клипом. 
Последовательность выполнения не нужна:
1. даже если алгоритм выполнения божественной задачи №1 использует удаление всех моментов неуказанных в ТЗ, НО при этом созданы 2 одинаковых секвенции с целостным клипом.
2. Если алгоритм выполнения божественной задачи №1 НЕ ИСПОЛЬЗУЕТ удаление всех моментов неуказанных в ТЗ.

Есть документ, который состоит из таблицы. В ней столбцы: "timecode", "to do". Эти столбцы есть в каждой строке таблицы. Есть еще дополнительные столбцы с текстом. Для каждой функции этот текст используется по-разному.

## 1. Дать доступ к значением из таблицы коду.

ТЗ будет написано в Google.таблице. 
С помощью встроенных в Google.таблицу выржений добавляются кавычки и запятые, чтобы можно было один раз копировать и код его посчитал списком.
какой-то текст -> [какой-то текст], 

- "00:54:14:00","removeFrom" ,"text1", "text2"
- "00:54:37:00","removeUntil","text1", "text2"
- "00:56:02:00","slideMif" ,"text1", "text2"
- "00:56:33:00","tiserFrom","text1", "text2"
- "01:01:35:00","tiserUntil","text1", "text2"

Далее копируются таблица с помощью "ctrl+c" и вставляется в массив "pesotcnitsa". Из-за того, что у каждого элемента есть квадратные скобочки и запятые между ними, то он считает элементы той таблицы, как элементы массива.

У нас есть список с элементами таблицы в виде массива. Но даже в виде массива мы можем получить доступ к любому столбцу и строке изначальной таблице. 
Обращаемся к индексу = b + 4*n. (где, b - это столбец (от 0 до 3), а "n" это номер строки (измняется от 0 до "суммы всех строк" (= "количество всех элементов в массиве"/4 )) 
Так например, 0 + 4*0 будет первая столбец и первая строка; 0 + 4*4 будет первый столбец и пятая строка; 0 + 4*12 бдует первый столбец и тринадцатая строка
А вот 1 + 4*12 будет второй столбец и тринадцатая строка; 2 + 4*12 будет третий столбец и тринадцатая строка. 

P.s. Самый первый список называется pesotcnitsa

Проблема: таким способом не получается копировать 
1. новые строчки, т.к. в случае новых строчек в таблице, к ней добавляются кавычки. Но к тексту и так добавляются кавычки и их получается 2Х. Но в тексте, где нет новых строчек, то там не добавляются кавычки. А отделить с помощью выржанеий, что если у ячейки есть новые строчки, то ей кавычки не нужны, я пока не умею.
2. Не передается жирность текста.

А это довольно весомый критерий. Нужно как-то иначе получать доступ к ячейкам и при этом копировать и жирность и новые строчки.

## 2. Распределение на группы (возможно стоит убрать, т.к. можно обращаться к нужному элементу таблицы из общего списка)

Хоть мы и можем обратиться к любому элементу таблицы, но рассортируем эти элементы в отдельный массив под каждую Божественную функцию

    1.1 Цикл: Получить доступ к элементу массива pesotcnitsa [1 + 4*n]; пока n != количеству всех элементов массива/4; после выполнение подпунктов n+1. (Т.е. так мы получаем доступ к элементу строки n второго столбца). 
        1.1.1 Если значение элемента равно "tiserUntil", "tiserFrom", то сохранить все значения строки, к которой был получен доступ, и добавить в массив arrayTiser.
        1.1.2 Если значение элемента равно "removeUntil", "removeFrom", "start", "end", то сохранить все значения строки, к которой был получен доступ, и добавить в массив arrayRemove.
        1.1.3 Если значение элемента равно "alert", то сохранить все значения строки, к которой был получен доступ, и добавить в массив arrayAlert.
        1.1.4 Если значение элемента равно любому иному, то сохранить все значения строки, к которой был получен доступ, и добавить в массив arraySlide.

## 3. Приводим таймкоды в читаемый вид для функций: убрать знак ":" и где-то перевести в тики (можно объединить с циклом выше)

3.1 
    3.1.1 Цикл: Получить доступ к элемнту массива arrayTiser [0 + 4*n]; пока n != количеству всех элементов массива/4; после выполнение подпунктов n+1.
        3.1.1.1 Убрать знак ":" из элемента.
    3.1.2 Цикл: Получить доступ к элемнту массива arrayRemove [0 + 4*n]; пока n != количеству всех элементов массива/4; после выполнение подпунктов n+1.
        3.1.2.1 Убрать знак ":" из элемента.
    3.1.3 Цикл: Получить доступ к элемнту массива alert [0 + 4*n]; пока n != количеству всех элементов массива/4; после выполнение подпунктов n+1.
        3.1.3.1 Убрать знак ":" из элемента.
    3.1.4 Цикл: Получить доступ к элемнту массива arraySlide [0 + 4*n]; пока n != количеству всех элементов массива/4; после выполнение подпунктов n+1.
        3.1.4.1 Убрать знак ":" из элемента.
        3.1.4.2 Нужно перевести в секунды * тики, которые равны 254016000000:
            3.1.4.2.1 Преобразуем строку в число
            3.1.4.2.2 ((Первые 2 цифры * 60 * 60) + (Вторые 2 цифры * 60) + (третие 2 цифры)) * 254016000000 (тик)
            3.1.4.2.3 Преобразуем цифры в строку

P.s. Если вставить этот пункт до разделение на отдельные Божественные функции, то можно упростить
    3.1.1 Цикл: Получить доступ к элементу массива pesotcnitsa = [0 + 4*n]; пока n != количеству всех элементов массива/4; после выполнение подпунктов n+1.
        3.1.1.1 Если элемент массива равен "tiserUntil" ИЛИ "tiserFrom" ИЛИ "removeUntil" ИЛИ "removeFrom" ИЛИ "start" ИЛИ "end" ИЛИ "alert" (все кроме слайдов) то убрать знак ":" из элемента.
        3.1.1.2 Иначе
            3.1.1.2.1 Преобразуем строку в число
            3.1.1.2.2 ((Первые 2 цифры * 60 * 60) + (Вторые 2 цифры * 60) + (третие 2 цифры)) * 254016000000 (тик)
            3.1.1.2.3 Преобразуем цифры в строку

            
~~3.2 Отсортировать таймкоды от большего к меньшему.~~ 
    ...

## 4. Удаление

ПРЕДИСЛОВИЕ

Я могу удалить клип с помощью функции sequence.razor, которая делит один клип на 2 других, и функции ".clips[].remove(0,0)", которая удаляет клип с порядковым номером n.
НО представим ситуацию, что на клипе уже есть разрезы |_____1_____|~~~~~~|_____2_____| и мне нужно вырезать момент. (где "|" - граница клипа, "___" - сам склип, "~" - пустота)

Если я сделаю разрез на первом клипе, то мне нужно будет удалить клип под номером [1]. |_0_|_1_|__2__|~~~~~~|_____3_____|
Если на втором, то клип под номером [2]. |___0___|~~~~~~|__1__|_2_|__3__|. 

В этой ситуации можно удалить не тот клип или нужно добавлять новую переменную-(ые), которые будут следить сколько разрезов было сделано до. Чтобы избежать неопределнности какой номер нужно удалить, я решил удалять последовательно. Т.е. начиная от самого начала и двигаясь к концу. Отсюда необходимость сортировать список, начиная от самых маленьких таймкодов, до самых больших.

Функция работает исходя из того, что:
1. Таймкоды в списке идут от меньшего к большему, т.е. последовательно.  
2. всегда первый таймкод это начало клипа, потом идут таймкоды, чтобы удалить промежутки, и в конце всегда есть последний таймкод, который обозначает конец клипа. Иначе будет ошибка,т.к. в начале и в конце нужно сделать один разрез, а в промежутках - 2.

КОНЕЦ ПРЕДИСЛОВИЯ

    Функция remove () {
        4.1 "Start"
            4.1.1 вызывать функцию библиотеки "QE" sequence.razor(элемент с таймкодом списка arrayRemove под номером [0])
            4.1.2 С помощью функции ".clips[].remove(0,0)" удалить клип с порядковым номером=индексом [0].  
        4.2 "Промежутки"
            4.2.1 a = 1;
            4.2.2 Цикл: n = 1; пока n != (arrayRemove.length/4) - 1; n++;
            4.2.2.1 Если n нечетное число, т.е. n =% 1, то вызывать функцию библиотеки "QE" sequence.razor(элемент с таймкодом списка arrayRemove под номером [0 + 4*n]) 
            4.2.2.2 Если n четное число, т.е. n =% 0, то 
                4.2.2.2.1 вызывать функцию библиотеки "QE" sequence.razor(элемент с таймкодом списка arrayRemove под номером [0 + 4*n]) 
                4.2.2.2.2 с помощью функции ".clips[].remove(0,0)" удалить клип с порядковым номером [n-a]
                4.2.2.2.3 a++
        4.3 "End"
            4.3.1 n++ 
            4.3.2 вызывать функцию библиотеки "QE" sequence.razor(элемент с таймкодом списка arrayRemove под номером [0 + 4*n]) 
            4.3.2 С помощью функции ".clips[].remove(0,0)" удалить клип с порядковым номером=индексом [n-a]. 
    }

    Всю функцию можно объединить внутри цикла 4.2, но тогда нужна добавлять к нечетности еще и аргумент цифр.

    Иллюстрация для лучшего понимания:
    |________________________________________| - изначальный клип
    0_|_____1________________________________| - n = 0; делаем начало видео.             1 порез.   Нужно удалить клип под номером 0. n нужно уменьшить на 0
    ~~|__0_|_1_|___2_________________________| - n = 1; n = 2; вырезали момент из клипа. 2 пореза.  Нужно удалить клип под номером 1. n нужно уменьшить на 1
    ~~|_0__|~~~|___1___|_2_|___3_____________| - n = 3; n = 4; вырезали момент из клипа. 2 пореза.  Нужно удалить клип под номером 2. n нужно уменьшить на 2
    ~~|_0__|~~~|___1___| ~ |__2___|_3_|_4____| - n = 5; n = 6; вырезали момент из клипа. 2 пореза.  Нужно удалить клип под номером 3. n нужно уменьшить на 3
    ~~|_0__|~~~|___1___| ~ |__2___| ~ |_3_|4_| - n = 7; сделали конец видео.             1 порез.   Нужно удалить клип под номером 4. n нужно уменьшить на 4  


    |~0~|__1___|~2~|___3__|~4~|__5__|~6~|___7___|~8~|_9_|~10~|_____11______|~12~|
    Оказывается можно и не соблюдать последовательность, если сначала сделать все разрезы, а потом удалить ненужное. Как видно то, что нужно удалить будет под четными номерами.
    Но нужно удалять с конца, чтобы не менялись порядковые номера клипов.

        если 0 порезов, то тольк 1 клип
        если 2 порезов, то только 3 клипа
        если 4 пореза , то только 5
        если 6 пореза, то только 7
        => (arrayRemove.length/4), т.е. количество строк=порезов, будет равно порядковому номеру последнего клипа. 
    |__|______|__|___|__|____|___|

Теперь функция remove работает с таймкодами в любой последовательности и пункт 3.2 сортировки не нужен. 

Ограничения функции: разрезов должно быть четное количество. 
1. Поэтому не предполагается, что будет разрез от начала клипа до точки отличной конца клипа и при этом не будет разреза от какой-то точки отличной от начала до конца клипа. верно и обратное. (т.е. ошибка если есть начало и нет конца клипа, или наоборот: нет начала, но есть конец клипа. Должно быть и начало и конец или не должно быть их обоих. Но чаще всего в разметке указывается начало и конец видео)
2. В видео должно быть начало и конец. Иначе ошибка.

Функция remove () {
    4.1 Цикл for (n = 0; пока n != (arrayRemove.length/4); n++) {
            вызывать функцию библиотеки "QE" sequence.razor(элемент с таймкодом списка arrayRemove под номером [0 + 4*n])
    }
    4.2 Цикл for (n = (arrayRemove.length/4); пока n != -1; n = n - 1) {
            Если n четное, т.е. n % = 0, то с помощью функции ".clips[].remove(0,0)" удалить клип с порядковым номером=индексом [n].
        }
}


## 5. Вырезать клип

    |__0___|~1~|___2__|~3~|__4__|~5~|___6___|~7~|_8_|~9~|_____10______|

Ограничения функции: разрезов должно быть четное количество. 
1. Поэтому не предполагается, что будет разрез от начала клипа до точки отличной конца клипа.
2. Аналогично: не предполагается, что будет разрез от какой-то точки отличной от начала до конца клипа.

Функция tiser () {
    4.1 Цикл for (n = 0; пока n != (arrayRemove.length/4); n++) {
            вызывать функцию библиотеки "QE" sequence.razor(элемент с таймкодом списка arrayRemove под номером [0 + 4*n])
    }
    5.2 Цикл for (n = (arrayRemove.length/4); пока n != -1; n = n - 1) {
            Если n четное, т.е. n % = 0, то с помощью функции ".clips[].remove(0,0)" удалить клип с порядковым номером=индексом [n].
        }
}

## 4. Нешаблонный 

...


## 5. Вставить

Проблема: слайдам нельзя вставлять жирность. Нужно пробовать как-то усовершенствовать имитацию жирности, чтобы это омно было делать автоматом. Или делать слайды в самом АЕ и там писать скрипты.
(хотя можно пока не реализовыывать) Проблема: нужно анимировать ключи для слайдов, чтобы они отъезжали в сторону. 

Для конечного слайда нужно высчитывать конец клипа - сколько-то кадров, чтобы он правильно по таймингу вставлялся.

//____________________________________
Весь код
//____________________________________
    
    var pesochnitsa = [  
    "00:47:43:00",	"start",	'',	"",
    "00:49:43:00",	"slideMif",	'Сотрудников учить не надо, они сами всему могут обучиться, надо просто попросить / поставить задачу.',	"str",
    "00:54:14:00",	"removeFrom",	'ну перейдем',	"empty",
    "00:54:37:00",	"removeUntil",	'да-да-да',	"empty",
    "00:55:22:00",	"slideRight",	'Вспомните ситуации из вашей школьной или университетской жизни, когда педагоги понадеялись на то, что школьник или студент что-то сделает. А тот забыл (или было лень), из-за чего у всех появились проблемы. Опишите этот случай в комментарии под нашим роликом.',	"empty",
    "00:56:02:00",	"removeFrom",	'давайте',	"",
    "00:56:33:00",	"removeUntil",	'сформулирую',	"",
    "00:56:33:00",	"slideMif",	'Креативные кадры обучать не надо – если их обучить, они перестанут креативить.',	"empty",
    "01:01:04:00",	"slideRight",	'Вспомните ситуацию, когда люди самооправдывали креативностью свою собственную лень, раздолбайство и нежелание работать по технологии / инструкциям. Опишите эти кейсы в комментариях под этим видео.',	"empty",
    "01:01:35:00",	"removeFrom",	'Юлия',	"empty",
    "01:01:45:00",	"removeUntil",	'Юлия',	"empty",
    "01:01:45:00",	"slideMif",	'Взрослых учить – только портить, мол, они уже не особо обучаемы, зачем этим заниматься.',	"empty",
    "01:07:28:00",	"Alert",	'empty',	"empty",
    "01:08:44:00",	"slideRight",	'Приведите в комментариях под этим роликом примеры профессий, в которых можно 10 лет ничему не учиться и оставаться востребованным специалистом. Наш главный вопрос: есть ли такие профессии вообще?',	"empty",
    "01:09:15:00",	"removeFrom",	'да, давайте',	"empty",
    "01:09:40:00",	"removeUntil",	'озвучу',	"empty",
    "01:09:40:00",	"slideMif",	'Корпоративный тренер – делает всё сам (одиночка).',	"empty",
    "01:15:34:09",	"Alert",	'',	"empty",
    "01:16:10:00",	"slideRight",	'Приведите примеры профессий, которые на первый взгляд – делают всё сами, но на самом деле там работает целая команда. Примеры, которые мы уже привели: корпоративный тренер, архитектор. Продолжите этот ряд в комментариях под видео.',	"empty",
    "01:17:25:00",	"removeFrom",	'ну можем',	"empty",
    "01:17:49:00",	"removeUntil",	'хорошо',	"empty",
    "01:17:49:00",	"slideMif",	'Сотрудников надо не учить, а мотивировать.',	"empty",
    "01:19:36:00",	"removeFrom",	'добавите',	"",
    "01:20:09:00",	"removeUntil",	'неумение работать',	"",
    "01:21:45:00",	"slideRight",	'Приведите в комментариях под этим видео примеры из личного опыта или опыта знакомых, когда мотивация – есть, но человек всё равно ничего не делает. И почему так происходит?',	"empty",
    "01:22:14:00",	"removeFrom",	'так у нас',	"empty",
    "01:22:30:00",	"removeUntil",	'секунду',	"empty",
    "01:22:30:00",	"slideMif",	'Обучение сотрудников – теория, тесты, лекции и семинары для повышения квалификации.',	"empty",
    "01:24:53:00",	"slideRight",	'Приведите в комментариях под этим роликом истории из жизни, когда человек преисполнился в познании теории, но потом на практике оказывалось, что он ничего не умеет. И опишите как это негативно повлияло на жизнь и карьеру этого человека?',	"empty",
    "01:25:24:00",	"removeFrom",	'так и последний',	"empty",
    "01:26:24:00",	"removeUntil",	'7-й миф',	"empty",
    "01:26:25:00",	"slideMif",	'«Вдохновители», стояние на гвоздях, медитация – это инструменты тренинга.',	"empty",
    "01:27:25:00",	"slideRight",	'Приведите в комментариях под этим роликом примеры «псевдо-тренингового шаманства», с которым вы или ваши знакомые сталкивались на работе. - В чем оно конкретно выражалось? - К каким последствиям приводило?',	"empty",
    "01:29:52:00",	"removeFrom",	'вот у нас',	"empty",
    "01:30:10:00",	"removeUntil",	'тишина',	"empty",
    "01:30:10:00",	"Alert",	'ВЫВОДЫ',	"empty",
    "01:31:16:00",	"Alert",	'empty',	"empty",
    "01:32:07:00",	"removeFrom",	'да и у нас',	"",
    "01:32:31:00",	"removeUntil",	'еще раз',	"",
    "01:33:03:00",	"Alert",	'',	"empty",
    "01:33:56:00",	"removeFrom",	'хорошо, спасибо',	"",
    "01:34:26:00",	"removeUntil",	'еще раз',	"",
    "01:34:40:00",	"slideRight",	'1) Теперь вам известны мифы. Но чтобы они перестали влиять на ваши решения, одного только знания мало. Для этого мы и даем задачи. Вы можете их решить и гораздо глубже понять то, о чем рассказывалось в ролике. Наши видео – инструмент и практическая возможность для вашего развития. А не просто очередной разговор о профессиях.',	"empty",
    "01:34:59:00",	"slideRight",	'2) В одиночку – тяжело. Если вы хотите: - Найти единомышленников - Вместе делать крутые проекты - Вместе строить ваше будущее То проект YouBe поможет вам с этим. Пишите по контактам, которые указаны в описании. Мы вам ответим и всё расскажем.',	"empty",
    "01:35:40:00",	"Alert",	'empty',	"empty",
    "01:35:42:00",	"end",	'',	"",
    "00:54:14:00",	"tiserFrom",	'',	"",
    "00:54:37:00",	"tiserUntil",	'',	"",
    "00:56:02:00",	"tiserFrom",	'',	"",
    "00:56:33:00",	"tiserUntil",	'',	"",
    "01:01:35:00",	"tiserFrom",	'',	"",
    "01:01:45:00",	"tiserUntil",	'',	"",
    "01:09:15:00",	"tiserFrom",	'',	"",
    "01:09:40:00",	"tiserUntil",	'',	"",
    "01:17:25:00",	"tiserFrom",	'',	"",
    "01:17:49:00",	"tiserUntil",	'',	"",
    "01:19:36:00",	"tiserFrom",	'',	"",
    "01:20:09:00",	"tiserUntil",	'',	"",
    "01:22:14:00",	"tiserFrom",	'',	"",
    "01:22:30:00",	"tiserUntil",	'',	"",
    "01:25:24:00",	"tiserFrom",	'',	"",
    "01:26:24:00",	"tiserUntil",	'',	"",

    ] 
    var chetchik = 0;
    var arrayRemove = [];
    var arrayTiser = [];
    var arraySlide = [];
    var arrayAlert = [];

    app.enableQE();
    var project = app.project;
    var sequence = project.activeSequence;
    var qeSequence = qe.project.getActiveSequence(0);

    var chetchik = 0
    var removeEnd = 0;
    var removeStart = 0;

    var indexBinUsullaySlides
    var nomerSlideRight = 1;
    var nomerSlidePlashka = 1;
    var chetchikRootItem = 0;

Этап привдение таймкодов в читаемый вид и сортировки функций.

    function timecodeToRead() {
        for (chetchik = 0; chetchik < pesochnitsa.length/4; chetchik++) {
            if (pesochnitsa[1+chetchik*4] == "tiserUntil"|| pesochnitsa[1+chetchik*4] == "tiserFrom" || pesochnitsa[1+chetchik*4] == "removeFrom" || pesochnitsa[1+chetchik*4] == "removeUntil" ||
            pesochnitsa[1+chetchik*4] == "start" || pesochnitsa[1+chetchik*4] == "end" || pesochnitsa[1+chetchik*4] == "Alert") {
                pesochnitsa[0+chetchik*4] = pesochnitsa[0+chetchik*4].replace(/[^0-9]/g,"");
            } else {
                pesochnitsa[0+chetchik*4] = String(254016000000 * (Number(pesochnitsa[0+chetchik*4].slice(0,2)) * 60 * 60 + Number(pesochnitsa[0+chetchik*4].slice(3,5)) * 60 + Number(pesochnitsa[0+chetchik*4].slice(6,8))));
            }
        }
    }

    function sort() {
        // Счетчик показывает на какой строке таблице он сейчас находится. Например если счетчик = 0, то он на первой строке. Если счетчик = 10, то он на 11 стрке.
        for (chetchik =0; chetchik < pesochnitsa.length/4; chetchik++) {
            if (pesochnitsa[1+chetchik*4] == "removeFrom" || pesochnitsa[1+chetchik*4] == "removeUntil" || pesochnitsa[1+chetchik*4] == "start" || pesochnitsa[1+chetchik*4] == "end") {
                arrayRemove.push(pesochnitsa[0 + chetchik*4], pesochnitsa[1 + chetchik*4], pesochnitsa[2 + chetchik*4], pesochnitsa[3 + chetchik*4]);
            } else if (pesochnitsa[1+chetchik*4] == "tiserFrom" || pesochnitsa[1+chetchik*4] == "tiserUntil") {
                arrayTiser.push(pesochnitsa[0 + chetchik*4], pesochnitsa[1 + chetchik*4], pesochnitsa[2 + chetchik*4], pesochnitsa[3 + chetchik*4]);
            } else if (pesochnitsa[1+chetchik*4] == "Alert") {
                arrayAlert.push(pesochnitsa[0 + chetchik*4], pesochnitsa[1 + chetchik*4], pesochnitsa[2 + chetchik*4], pesochnitsa[3 + chetchik*4]);
            } else {
                arraySlide.push(pesochnitsa[0 + chetchik*4], pesochnitsa[1 + chetchik*4], pesochnitsa[2 + chetchik*4], pesochnitsa[3 + chetchik*4])
            }
        }
    }


Функция удалить промежутки

    function removeClip () {
        sequence.videoTracks[0].clips[chetchik].remove(0,0);
        sequence.audioTracks[0].clips[chetchik].remove(0,0);
    }
    function remove () {
        removeEnd = 0;
        removeStart = 0;
        for (chetchik = 0; chetchik < arrayRemove.length/4; chetchik++) {
            qeSequence.razor(arrayRemove[0 + 4*chetchik].toString()); 
            if (arrayRemove[1 + 4*chetchik] == "end") {removeEnd = 1}
            if (arrayRemove[1 + 4*chetchik] == "start") {removeStart = 1}
        }

        for (chetchik = arrayRemove.length/4; chetchik != -1; chetchik = chetchik - 1) {
            if (removeEnd == 1 && removeStart == 1) {
                if (chetchik % 2 == 0) {removeClip ()}
            } else if (removeEnd == 0 && removeStart == 0) {
                if (chetchik % 2 == 1) {removeClip ()}
            } else if (removeEnd == 0 && removeStart == 1) {
                if (chetchik % 2 == 0) {removeClip ()}
            } else if (removeEnd == 1 && removeStart == 0) {
                if (chetchik % 2 == 1) {removeClip ()}
            }
        }
    }

Функция тизер

    function tiser () {
        lastSequenceName = project.activeSequence.name;
        copySequenceName = project.activeSequence.name + " Copy";
        sequence.clone();
        sequence.name = lastSequenceName + " Тизер";
        for (chetchik = 0; chetchik < arrayTiser.length/4; chetchik++) {
            qeSequence.razor(arrayTiser[0 + 4*chetchik].toString());
        }
        for (chetchik = arrayTiser.length/4; chetchik != -1; chetchik = chetchik - 1) {
            if (chetchik % 2 == 0) {
                removeClipWithoutEmpty ();
            }
        }
        for (chetchik = 0; app.project.sequences[chetchik].name != copySequenceName; chetchik++) {
        }
        app.project.sequences[chetchik].name = lastSequenceName;
        app.project.openSequence(app.project.sequences[chetchik].sequenceID);
    }

Функция вставить слайды

    function mogrtImport(file) {
        sequence.importMGT(file.fsName, String(arraySlide[0 + chetchik*4]), 2, 2);
        }
    function takeSlide () {
        for (chetchik = 0; chetchik < arraySlide.length/4; chetchik++) {
            if (arraySlide[1 + 4 * chetchik] == "slideRight") {
                mogrtImport(File("G:/Монтаж/Ливрезон/ЛИМ/MOGRT LIM/SlideRight.mogrt"));
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[1].setValue(arraySlide[2 + 4 * chetchik]);
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[2].setValue(arraySlide[2 + 4 * chetchik]);
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[0].setValue("задача №" + nomerSlideRight+ ":");
                nomerSlideRight++;
            } else if (arraySlide[1 + 4 * chetchik] == "slideMif") {
                mogrtImport(File("G:/Монтаж/Ливрезон/ЛИМ/MOGRT LIM/SlideDown.mogrt"));
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[3].setValue(arraySlide[2 + 4 * chetchik]);
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[2].setValue(String(nomerSlidePlashka));
                nomerSlidePlashka++;
            } else if (arraySlide[1 + 4 * chetchik] == "slideBook") {
                mogrtImport(File("G:/Монтаж/Ливрезон/ЛИМ/MOGRT LIM/SlideDown.mogrt"));
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[0].setValue(1);
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[1].setValue(7);
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[3].setValue(arraySlide[2 + 4 * chetchik]);
             } else if (arraySlide[1 + 4 * chetchik] == "slideEmpty") {
                mogrtImport(File("G:/Монтаж/Ливрезон/ЛИМ/MOGRT LIM/SlideDown.mogrt"));
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[0].setValue(2);
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[3].setValue(arraySlide[2 + 4 * chetchik]);
            } else if (arraySlide[1 + 4 * chetchik] == "TransitionBlock") {
                mogrtImport(File("G:/Монтаж/Ливрезон/ЛИМ/MOGRT LIM/TransitionBlock.mogrt"));
                sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[0].setValue(arraySlide[2 + 4 * chetchik]);
                if (arraySlide[chetchik][3] == "") {
                    sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[2].setValue(0);
                } else {
                    sequence.videoTracks[2].clips[chetchik].getMGTComponent().properties[1].setValue(arraySlide[chetchik][3]);
                }
            } else if (arraySlide[1 + 4 * chetchik] == "Subscribe") {
                for (chetchikRootItem = 0; app.project.rootItem.children[chetchikRootItem].name != "Usullay slides"; chetchikRootItem++) {
                    }
                indexBinUsullaySlides = chetchikRootItem;
                for (chetchikRootItem = 0; app.project.rootItem.children[indexBinUsullaySlides].children[chetchikRootItem].name != "Подписаться.mov"; chetchikRootItem++) {
                    }
                sequence.videoTracks[2].overwriteClip(app.project.rootItem.children[indexBinUsullaySlides].children[chetchikRootItem], arraySlide[chetchik][0]);
            } else if (arraySlide[1 + 4 * chetchik] == "Дисклэймер") {
                for (chetchikRootItem = 0; app.project.rootItem.children[chetchikRootItem].name != "Usullay slides"; chetchikRootItem++) {
                    }
                indexBinUsullaySlides = chetchikRootItem;
                for (chetchikRootItem = 0; app.project.rootItem.children[indexBinUsullaySlides].children[chetchikRootItem].name != "Дисклэймер.mov"; chetchikRootItem++) {
                    }
                sequence.videoTracks[2].overwriteClip(app.project.rootItem.children[indexBinUsullaySlides].children[chetchikRootItem], arraySlide[chetchik][0]);
            } else if (arraySlide[1 + 4 * chetchik] == "Имя Илья") {
                for (chetchikRootItem = 0; app.project.rootItem.children[chetchikRootItem].name != "Usullay slides"; chetchikRootItem++) {
                    }
                indexBinUsullaySlides = chetchikRootItem;
                for (chetchikRootItem = 0; app.project.rootItem.children[indexBinUsullaySlides].children[chetchikRootItem].name != "Имя Илья.mov"; chetchikRootItem++) {
                    }
                sequence.videoTracks[2].overwriteClip(app.project.rootItem.children[indexBinUsullaySlides].children[chetchikRootItem], arraySlide[chetchik][0]);
            } else if (arraySlide[1 + 4 * chetchik] == "Transition") {
                for (chetchikRootItem = 0; app.project.rootItem.children[chetchikRootItem].name != "Usullay slides"; chetchikRootItem++) {
                    }
                indexBinUsullaySlides = chetchikRootItem;
                for (chetchikRootItem = 0; app.project.rootItem.children[indexBinUsullaySlides].children[chetchikRootItem].name != "Transition shape.mov"; chetchikRootItem++) {
                    }
                sequence.videoTracks[2].overwriteClip(app.project.rootItem.children[indexBinUsullaySlides].children[chetchikRootItem], arraySlide[chetchik][0]);
            }
        }
    }





