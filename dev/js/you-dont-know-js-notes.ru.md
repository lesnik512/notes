# Вы не знаете JS: Область видимости и замыкания
# Глава 1: Что такое область видимости?

Область видимости — это набор правил, которые определяют где и как переменная (идентификатор) могут быть найдены. Этот поиск может осуществляться для целей присваивания значения переменной, которая является LHS (left-hand-side) ссылкой, или может осуществляться для целей извлечения ее значения, которое является RHS (right-hand-side) ссылкой.

LHS-ссылки являются результатом операции присваивания. Присваивания, связанные с *Областью видимости*, могут происходить либо с помощью операции `=`, либо передачей аргументов (присваиванием) параметрам функции.

JavaScript *Движок* перед выполнением сначала компилирует код, и пока он это делает, он разбивает операторы, подобные `var a = 2;` на два отдельных шага:

1. Первый, `var a`, чтобы объявить ее в *Область видимости*. Это выполняется в самом начале, до исполнения кода.

2. Позже, `a = 2` ищет переменную (LHS-ссылку) и присваивает ей значение, если находит.

Оба поиска ссылок LHS и RHS начинаются в текущей выполняющейся *Области видимости* и если нужно (т.е. они не нашли что искали в ней), они работают с их более высокими вложенными *Областями видимости*, с одной областью (этажом) за раз, ища идентификатор, пока не доберутся до глобальной (верхний этаж) и не остановятся, вне зависимости от результата поиска.

Невыполненные RHS-ссылки приводят к выбросу `ReferenceError`. Невыполненные LHS-ссылки приводят к автоматической, неявно созданной переменной с таким именем (если не включен "Строгий режим"), либо к `ReferenceError` (если включен "Строгий режим").

# Глава 2: Лексическая область видимости

Лексическая область видимости означает, что область видимости определена решениями о том, где объявляются функции на стадии написания кода. Фаза разбиения на лексемы при компиляции фактически способна узнать где и как объявлены все идентификаторы, и таким образом предсказать как их будут искать во время выполнения.

Два механизма в JavaScript могут "обмануть" лексическую область видимости: `eval(..)` и `with`. Первый может менять существующую лексическую область видимости (во время выполнения) исполняя строку "кода", в которой есть одно или несколько объявлений. Второй по сути создает целую новую лексическую область видимости (снова во время выполнения) интерпретируя ссылку на объект *как* "область видимости", а свойства этого объекта как идентификаторы этой области.

Недостаток этих механизмов в том, что они лишают смысла возможность *Движка*  выполнять оптимизации во время компиляции, принимающие во внимание поиск в области видимости, так как *Движок* должен пессимистически предположить, что такие оптимизации будут неправильными. Код *будет* выполняться медленнее в результате использования любой из этих возможностей. **Не используйте их!**


# Глава 3: Область видимости: функции против блоков

Функции — самые распространенные единицы области видимости в JavaScript. Переменные и функции, которые объявляются внутри другой функции, по существу "скрыты" от любой из окружающих "областей видимости", что является намеренным принципом разработки хорошего ПО.

Но функции — отнюдь не только единицы области видимости. Блочная область видимости ссылается на идею, что переменные и функции могут принадлежать произвольному блоку (обычно, любой паре `{ .. }`) кода, нежели только окружающей функции.

Начиная с ES3, в структуре `try/catch` есть блочная область видимости в выражении `catch`.

В ES6 представлено ключевое слово `let` (родственница ключевого слова `var`), чтобы позволить объявления переменных в любом произвольном блоке кода. `if (..) { let a = 2; }` объявит переменную `a`, которая фактически похитит область видимости блока `{ .. }` в `if` и присоединит себя к ней.

Хоть некоторые похоже и верят в это, но блочную область видимости не следует использовать как полную замену функциональной области видимости `var`. Обе функциональности сосуществуют вместе, а разработчики могут и должны использовать обе техники области видимости: функциональную и блочную, в соответствующих местах, чтобы создавать лучший, более читаемый/обслуживаемый код.

[Принцип наименьших привилегий](http://en.wikipedia.org/wiki/Principle_of_least_privilege)

# Глава 4: Поднятие переменных (Hoisting)

У нас есть соблазн смотреть на `var a = 2;` как на один оператор, но  *Движок* JavaScript видит это по-другому. Он видит `var a` и `a = 2` как два отдельных оператора, первый — как задачу фазы компиляции, а второй — как задачу фазы выполнения.

Это приводит к тому, что все объявления в области видимости, независимо от того где они появляются, обрабатываются *первыми*, до того, как сам код будет выполнен. Можно мысленно представить себе это как объявления (переменных и функций), "переезжающие" в начало их соответствующих областей видимости, что мы называем "поднятие (hoisting)".

Сами объявления поднимаются, а присваивания, даже присваивания функциональных выражений, *не* поднимаются.

Остерегайтесь дублей объявлений, особенно смешанных обычных объявлений var и объявлений функций — вас будут поджидать неприятности!

# Глава 5: Замыкание области видимости

Похоже, что знания о замыкании полны предрассудков и суеверий как загадочный мир, стоящий особняком внутри JavaScript, который могут познать только самые храбрые души. Но на самом деле — это всего лишь стандартный и почти очевидный факт о том, как писать код в среде лексической области видимости, где функции являются значениями и могут свободно передаваться куда угодно.

**Замыкание — это когда функция может запомнить и иметь доступ к своей лексической области видимости даже тогда, когда она вызывается вне своей лексической области видимости.**

Замыкания могут сбить нас с толку, например в циклах, если мы не озаботимся тем, чтобы распознавать их и то как они работают. Но они еще и являются весьма мощным инструментом, дающим доступ к шаблонам, таким как *модули* в их различных формах.

Модули требуют две ключевых характеристики: 1) внешнюю функцию-обертку, которую будут вызывать, чтобы создать закрытую область видимости 2) возвращаемое значение функции-обертки должно включать в себя ссылку на не менее чем одну внутреннюю функцию, у которой потом будет замыкание на внутреннюю область видимости обертки.

Теперь вы сможете заметить замыкания повсюду в своем существующем коде и у нас теперь есть возможность обнаруживать и использовать все их преимущества!

# Вы не знаете JS: This и Прототипы Объектов
# Глава 1: `this` (этот) или That (тот)?

`this` — это не ссылка функции на саму себя и это не ссылка на область видимости функции. `this` — это привязка, которая создается во время вызова функции, и на *что* она ссылается определяется тем, где и при каких условиях функция была вызвана.


# Глава 2: Весь `this` теперь приобретает смысл!


Определение привязки `this` для вызова функции требует поиска непосредственной точки вызова этой функции. Как уже выяснилось, к точке вызова могут быть применены четыре правила, в *именно таком* порядке  приоритета:

1. Функция вызвана с `new` (**привязка new**)? Раз так, то `this` — новый сконструированный объект.

    `var bar = new foo()`

2. Функция вызвана с `call` или `apply` (**явная привязка**), даже скрыто внутри *жесткой привязки* в `bind`? Раз так, `this` — явно указанный объект.

    `var bar = foo.call( obj2 )`

3. Функция вызвана с контекстом (**неявная привязка**), иначе называемым как владеющий или содержащий объект? Раз так, `this` является *тем самым* объектом контекста.

    `var bar = obj1.foo()`

4. В противном случае, будет `this` по умолчанию (**привязка по умолчанию**). В режиме `strict mode`, это будет `undefined`, иначе будет объект `global`.

    `var bar = foo()`

Остерегайтесь случайного/неумышленного вызова с применением правила *привязки по умолчанию*. В случаях, когда вам нужно "безопасно" игнорировать привязку `this`, "DMZ"-объект, подобный `ø = Object.create(null)`, — хорошая замена, защищающая объект `global` от непредусмотренных побочных эффектов.

Вместо четырех стандартных правил привязки стрелочные функции ES6 используют лексическую область видимости для привязки `this`, что означает, что они  заимствуют привязку `this` (какой бы она ни была) от вызова своей окружающей функции. Они по существу являются синтаксической заменой `self = this` в до-ES6 коде.

# Глава 3: Объекты

Объекты в JS имеют литеральную форму (вроде `var a = { .. }`) и конструкторную форму (вроде `var a = new Array(..)`). Литеральная форма почти всегда предпочтительнее, но конструкторная форма в некоторых случаях предлагает больше опций при создании.

Многие ошибочно заявляют, что «в JS всё является объектом», но это некорректно. Объекты -- это один из 6 (или 7, в зависимости от ваших взглядов) примитивных типов. Существуют подтипы объектов, в том числе `function`, а также подтипы со специальным поведением, наподобие `[object Array]`, представляющего внутреннее обозначение такого подтипа объекта, как массив.

Объекты -- это коллекции ключ-значение. Значения могут быть получены через свойства, посредством синтаксиса `.propName` или `["propName"]`. Вне зависимости от синтаксиса, движок вызывает встроенную стандартную операцию `[[Get]]` (и `[[Put]]` для установки значений), которая не только ищет свойство непосредственно в объекте, но и перемещается по цепочке `[[Prototype]]` (см. Главу 5), если свойство не найдено.

У свойств есть определенные характеристики, которыми можно управлять через дескрипторы свойств, такие как `writable` и `configurable`. В дополнение, мутабельностью объектов (и их свойств) можно управлять на разных уровнях иммутабельности, используя `Object.preventExtensions(..)`, `Object.seal(..)`, и `Object.freeze(..)`.

Свойства не обязательно содержат значения -- они могут быть также «свойствами доступа» с геттерами/сеттерами. Они могут быть *перечисляемыми* или нет, что влияет на их появление в итерациях цикла, например `for..in`.

Вы также можете перебирать **значения** структур данных (массивов, объектов и т.п.) используя синтаксис ES6 `for..of`, который ищет встроенный или самодельный объект `@@iterator`, содержащий метод `next()` для перебора значений по одному.

# Глава 4: Смешивая объекты "классов"

Классы - это шаблон кодирования. Многие языки предоставляют синтаксис, который позволяет проектировать класс-ориентированное программное обеспечение. JS также имеет похожий синтаксис, но он ведет себя **совсем иначе**, чем вы ожидаете от классов из этих других языков.

**Классы означают копии.**

Когда создаются традиционные классы, происходит копирование поведения от класса к экземпляру. Когда классы наследуются, также происходит копирование поведения от родителя к потомку.

Может показаться что полиморфизм (имеющий разные функции на нескольких уровнях цепочки наследования с одним и тем же именем) подразумевает относительную ссылку от дочернего элемента к родительскому, но это все еще просто результат поведения копирования.

JavaScript **автоматически не** создает копии (как подразумевают классы) между объектами.

Шаблон примеси (как явный, так и неявный) часто используется для *эмуляции* поведения копирования классов, но это обычно приводит к уродливому и хрупкому синтаксису, например явному псевдополиморфизму (`OtherObj.methodName.call(this, ...)`), что часто приводит к усложнению понимания и поддержки кода.

Явные примеси также не совсем совпадают с *копированием* классов, поскольку объекты (и функции!) дублируются только общими ссылками, а сами объекты/функции не дублируются. Не обратив внимания на такой нюанс вы получите источник множества недочетов.

В целом, фальшивые классы в JS часто устанавливают больше мин для будущего кодирования, вместо решения *реальных* проблем.

# Глава 5: Прототипы

## Неявное затемнение
```js
var anotherObject = {
  a: 2,
};

var myObject = Object.create(anotherObject);

anotherObject.a; // 2
myObject.a; // 2

anotherObject.hasOwnProperty("a"); // true
myObject.hasOwnProperty("a"); // false

myObject.a++; // ой, неявное затенение!

anotherObject.a; // 2
myObject.a; // 3

myObject.hasOwnProperty("a"); // true
```

При попытке обратиться к несуществующему свойству объекта внутренняя ссылка `[[Prototype]]` этого объекта задает дальнейшее направление поиска для операции `[[Get]]`. Этот каскад ссылок от объекта к объекту образует "цепочку прототипов" (чем то похожую на цепочку вложенных областей видимости) для обхода при разрешении свойства.

У обычных объектов есть встроенный объект `Object.prototype` на конце цепочки прототипов (похоже на глобальную область видимости при поиске по цепочке областей видимости), где процесс разрешения свойства остановится, если свойство не будет найдено в предыдущих звеньях цепочки. У этого объекта есть утилиты `toString()`, `valueOf()` и несколько других, благодаря чему все объекты в языке имеют доступ к ним.

Наиболее популярный способ связать два объекта друг с другом — использовать ключевое слово `new` с вызовом функции, что помимо четырех шагов (см. главу 2) создаст новый объект, привязанный к другому объекту.

Этим "другим объектом" является объект, на который указывает свойство `.prototype` функции, вызванной с `new`. Функции, вызываемые с `new`, часто называют "конструкторами", несмотря на то что они не создают экземпляры классов, как это делают _конструкторы_ в традиционных класс-ориентированных языках.

Хотя эти механизмы JavaScript могут напоминать "создание экземпляров классов" и "наследование классов" из традиционных класс-ориентированных языков, ключевое отличие в том, что в JavaScript не создаются копии. Вместо этого объекты связываются друг с другом через внутреннюю цепочку `[[Prototype]]`.

По множеству причин, среди которых не последнюю роль играет терминологический прецедент, "наследование" (и "прототипное наследование") и все остальные ОО-термины не имеют смысла, учитывая то, как _на самом деле_ работает JavaScript.

Более подходящим термином является "делегирование", поскольку эти связи являются не _копиями_, а делегирующими **ссылками**.

# Глава 6: Делегирование поведения

Классы и наследование — это один из возможных шаблонов проектирования, который вы можете *использовать* или *не использовать* в архитектуре вашего ПО. Большинство разработчиков считают само собой разумеющимся тот факт, что классы являются единственным (правильным) способом организации кода. Но в этой главе мы увидели другой, менее популярный, но весьма мощный шаблон проектирования: **делегирование поведения**.

Делегирование поведения предполагает, что все объекты находятся на одном уровне и связаны друг с другом делегированием, а не отношениями родитель-потомок. Механизм `[[Prototype]]` в JavaScript по своему замыслу является механизмом делегирования поведения. Это значит, что мы можем либо всячески пытаться реализовать механику классов поверх JS (см. главы 4 и 5), либо принять истинную сущность `[[Prototype]]` как механизма делегирования.

Если вы проектируете код, используя только объекты, это не только упрощает синтаксис, но и позволяет добиться более простой архитектуры кода.

**OLOO** (объекты, связанные с другими объектами) — это стиль кодирования, в котором объекты создаются и связываются друг с другом без абстракции классов. OLOO вполне естественным образом реализует делегирование поведения при помощи `[[Prototype]]`.

# Приложение А: `class` в ES6

`class` очень хорошо притворяется, что решает проблемы с паттерном класс/наследование в JS. Но на самом деле делает обратное: **он скрывает многие проблемы, но приносит другие, незаметные, но опасные**.

`class` способствует постоянной путанице с «классами» в JS, которая преследует язык около двух десятков лет. Во многом он вызывает больше вопросов, чем ответов и в целом чувствуется, что он противоестественно расположился над элегантной простотой механизма `[[Prototype]]`.

Итоги: в ES6 `class` затрудняет надёжное использование `[[Prototype]]` и скрывает самую важную особенность механизма объектов в JS -- **живое делегирование связей между объектами** -- не лучше ли рассматривать `class` как создающий больше проблем, чем решающий и просто отнести его к анти-паттерну?

На самом деле я не могу ответить за вас. Но я надеюсь, что эта книга для вас полностью раскрыла проблему на более глубоком уровне, чем когда-либо ранее и дала вам необходимую информацию чтобы *вы сами смогли ответить*.

# Вы не знаете JS: Типы и грамматика
# Глава 1: Типы

В JavaScript есть семь встроенных *типов*: `null`, `undefined`, `boolean`, `number`, `string`, `object`, `symbol`. Их можно определить с помощью оператора `typeof`.

Переменные не имеют типов, но их имеют значения переменных. Эти типы определяют внутреннее поведение значений.

Многие разработчики полагают, что «неопределённый» и «необъявленный» — это примерно одно и то же, но в JavaScript это совершенно разные вещи. `undefined` («неопределённый») — это значение, которое может содержать объявленная переменная. «Undeclared» («необъявленный») означает, что переменная не была объявлена.

JavaScript, к сожалению, в некотором роде отождествляет эти два термина, не только в сообщениях об ошибках («ReferenceError: a is not defined»), но также и в значении, возвращаемом `typeof`, являющемся `"undefined"` для обоих случаев.

Однако защита (предотвращение ошибки) в `typeof` при использовании с необъявленной переменной может быть полезна в некоторых случаях.

# Глава 2: Типы

In JavaScript, `array`s are simply numerically indexed collections of any value-type. `string`s are somewhat "`array`-like", but they have distinct behaviors and care must be taken if you want to treat them as `array`s. Numbers in JavaScript include both "integers" and floating-point values.

Several special values are defined within the primitive types.

The `null` type has just one value: `null`, and likewise the `undefined` type has just the `undefined` value. `undefined` is basically the default value in any variable or property if no other value is present. The `void` operator lets you create the `undefined` value from any other value.

`number`s include several special values, like `NaN` (supposedly "Not a Number", but really more appropriately "invalid number"); `+Infinity` and `-Infinity`; and `-0`.

Simple scalar primitives (`string`s, `number`s, etc.) are assigned/passed by value-copy, but compound values (`object`s, etc.) are assigned/passed by reference-copy. References are not like references/pointers in other languages -- they're never pointed at other variables/references, only at the underlying values.

# Глава 3: Стандартные встроенные объекты

JavaScript provides object wrappers around primitive values, known as natives (`String`, `Number`, `Boolean`, etc). These object wrappers give the values access to behaviors appropriate for each object subtype (`String#trim()` and `Array#concat(..)`).

If you have a simple scalar primitive value like `"abc"` and you access its `length` property or some `String.prototype` method, JS automatically "boxes" the value (wraps it in its respective object wrapper) so that the property/method accesses can be fulfilled.

# Вы не знаете JS: Асинхронность и Выполнение
# Глава 1: Асинхронность: Сейчас и Потом

A JavaScript program is (practically) always broken up into two or more chunks, where the first chunk runs *now* and the next chunk runs *later*, in response to an event. Even though the program is executed chunk-by-chunk, all of them share the same access to the program scope and state, so each modification to state is made on top of the previous state.

Whenever there are events to run, the *event loop* runs until the queue is empty. Each iteration of the event loop is a "tick." User interaction, IO, and timers enqueue events on the event queue.

At any given moment, only one event can be processed from the queue at a time. While an event is executing, it can directly or indirectly cause one or more subsequent events.

Concurrency is when two or more chains of events interleave over time, such that from a high-level perspective, they appear to be running *simultaneously* (even though at any given moment only one event is being processed).

It's often necessary to do some form of interaction coordination between these concurrent "processes" (as distinct from operating system processes), for instance to ensure ordering or to prevent "race conditions." These "processes" can also *cooperate* by breaking themselves into smaller chunks and to allow other "process" interleaving.