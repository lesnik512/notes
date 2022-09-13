```js

/*
Путь до поля объекта
Нужно написать функцию get. На вход функция принимает объект и путь до поля объекта.
Путь – это строка, разделенная точкой. Функция должна вернуть соответствующее поле объекта.
Запрашиваемое поле в объекте точно есть.
*/
// объект
const obj = {
    a: {
        b: {
            c: 'd'
        },
        e: 'f'
    }
};
// функция
function get(obj, path) {
    // код
}
// результаты вызовов
get(obj, 'a.b');   // { c : 'd' }
get(obj, 'a.b.c'); // 'd'
get(obj, 'a.e');   // 'f'

	

// split строки пути, проход по массиву ключей в цикле (без рекурсии, в идеале - через reduce), перезапись объекта


/*
Путь до поля объекта
Нужно написать функцию get. На вход функция принимает объект и путь до поля объекта.
Путь – это строка, разделенная точкой. Функция должна вернуть соответствующее поле объекта.
Запрашиваемое поле в объекте точно есть.
*/
// объект
const obj = {
    a: {
        b: {
            c: 'd'
        },
        e: 'f'
    }
};
// функция
function get(obj, path) {
    const keys = path.split('.');
    return keys.reduce((acc, el) => {
        const newEl = acc[el];
        if (!newEl) {
            throw new TypeError('No key in obj')
        }
        return newEl;
    }, obj);
}
// результаты вызовов
get(obj, 'a.b');   // { c : 'd' }
get(obj, 'a.b.c'); // 'd'
get(obj, 'a.e');   // 'f'


/*
Записная книга
Реализовать записную книгу, в которой существует 3 вида операций по номеру телефона:
добавление, поиск и удаление
*/
// хранилище данных
const store = 'TODO'; /* какую структуру выбрать */
// запись в хранилище
const reducer = () => {
    // код
};
// примеры вызовов и их результаты
reducer({type: 'add', payload: { phone: 71111111111, name: 'Алиса'}});
reducer({type: 'add', payload: { phone: 72222222222, name: 'Петя'}});
reducer({type: 'add', payload: { phone: 102, name: 'Полиция'}});
reducer({type: 'find', payload: { phone: 71111111111 }}); // Алиса
reducer({type: 'find', payload: { phone: 103 }}); // нет записи
reducer({type: 'find', payload: { phone: 102 }}); // Полиция
reducer({type: 'del', payload: { phone: 102 }});
reducer({type: 'find', payload: { phone: 72222222222 }}); // Петя
reducer({type: 'add', payload: { phone: 72222222222, name: 'Папа' }});
reducer({type: 'find', payload: { phone: 72222222222 }}); // Папа
reducer({type: 'del', payload: { phone: 102 }});
// хэш-мэп в качестве хранилища. хранение имени по телефону. не забыть фолбэк на "нет записи". если решает через список - важно, чтобы не забыл про перезапись по номеру телефона (Петя -> Папа) и про возврат имени, а не всего объекта. а ещё - спросить, можно ли проще (вдруг вспомнит про хэш-мэп)

/*
Записная книга
Реализовать записную книгу, в которой существует 3 вида операций по номеру телефона:
добавление, поиск и удаление
*/
// хранилище данных
const store = {
    
}; /* какую структуру выбрать */
// запись в хранилище
const reducer = (payload) => {
    const { phone } = payload;

    switch(type) {
        case 'add': {
            const { name } = payload;
            // Because phone is unique.
            store[phone] = name;
        }
        case 'find': {
            const userFound = store[phone];
            return userFound ? userFound : 'нет записи';
        }
        case 'del': {
            store[phone] = undefined;
        }
    }
};
// примеры вызовов и их результаты
reducer({type: 'add', payload: { phone: 71111111111, name: 'Алиса'}});
reducer({type: 'add', payload: { phone: 72222222222, name: 'Петя'}});
reducer({type: 'add', payload: { phone: 102, name: 'Полиция'}});
reducer({type: 'find', payload: { phone: 71111111111 }}); // Алиса
reducer({type: 'find', payload: { phone: 103 }}); // нет записи
reducer({type: 'find', payload: { phone: 102 }}); // Полиция
reducer({type: 'del', payload: { phone: 102 }});
reducer({type: 'find', payload: { phone: 72222222222 }}); // Петя
reducer({type: 'add', payload: { phone: 72222222222, name: 'Папа' }});
reducer({type: 'find', payload: { phone: 72222222222 }}); // Папа
reducer({type: 'del', payload: { phone: 102 }});


/*
Объединение и пересечение списков
Нужно написать 2 функции, каждая из которых будет принимать на вход по 2 массива с числами.
1-я должна производить объединение этих массивов, а 2-я - пересечение.
Пример:
1-й список: 1, 2, 2, 5, 7, 14
2-й список: 4, 6, 6, 7, 9, 14, 15
Объединение: 1, 2, 2, 4, 5, 6, 6, 7, 7, 9, 14, 14, 15
Пересечение: 7, 14
*/
//объединение, в идеале - должно быть в 1 проход (по длинному массиву). но допустимы и всякие вариации с concat или spread. пересечение - через разложение одного из массивов на хэш-мэп или Set. квадратичные алгоритмы - плохо
/*
Объединение и пересечение списков
Нужно написать 2 функции, каждая из которых будет принимать на вход по 2 массива с числами.
1-я должна производить объединение этих массивов, а 2-я - пересечение.
Пример:
1-й список: 1, 2, 2, 5, 7, 14
2-й список: 4, 6, 6, 7, 9, 14, 15
Объединение: 1, 2, 2, 4, 5, 6, 6, 7, 7, 9, 14, 14, 15
Пересечение: 7, 14
*/

const union = (a, b) => {
    return a.concat(b).sort((x1, x2) => x1 >= x2);
}

const cross = (a, b) => {
    result = [];
    for(i = 0; i < a.length; i++) {
        const elA = a[i];
        for(j = 0; j < b.length; j++) {
            const elB = b[j];
            if (elA === elB) {
                result.push(elB);
                break;
            }
        }
    }
    return result;
}



/*
Перезапросы при ошибках
Необходимо написать функцию, которая в качестве параметра принимает URL, а возвращает promise, в рамках которого
происходит асинхронный GET-запрос (через fetch) и возвращается некий результат (например, JSON).
Если во время запроса произошла ошибка, то функция должна попробовать сделать запрос ещё 5 раз.
Если в итоге запрос так и не удался, функция должна вернуть ошибку "Заданный URL недоступен".
*/

const poll = (url, callTimes = 5) => {
    if (callTimes === 0) {
        throw new Error('Заданный URL недоступен');
    }
    return fetch(url).then((res) => res.json()).catch(() => poll(url, callTimes - 1));

}



/*
Конвертер курсов валют
Необходимо написать React-компонент, который представлен двумя зависимыми полями ввода ("RUB" и "USD") и кнопкой между ними ("<->")
При изменении значения в 1-м поле - меняется значение в 2-м (если в поле "RUB" ввести 150, значение в поле "USD" должно стать 1)
При нажатии на кнопку - поля меняются местами (поле "RUB" становится 2-м, а поле "USD" - 1-м, и наоборот), и 2-е поле блокируется
Курс валют отличается в зависимости от направления конвертации (150 RUB = 1 USD, но 1 USD = 75 RUB)
*/
import React, { useCallback } from 'react';

const Converter: React.FC<> = () => {
    const [currency1, setCurrency1] = useState(0);
    const [currency2, setCurrency2] = useState(0);
    const [currencyName1, setCurrencyName1] = useState('RUB');
    const [currencyName2, setCurrencyName2] = useState('USD');

    // The *key* is current currencyName1.
    const converterCoefficients = {
        ['RUB'] = 1 / 150,
        ['USD'] = 75,     
    }

    const swapCurrencies = useCallback(() => {
        setCurrency1(currency2);
        setCurrency2(currency1);

        setCurrencyName1(currencyName2);
        setCurrencyName2(currencyName1);
        
    }, [currency1, currency2]);

    const onCurrencyChange = useCallback((e) => {
        setCurrency1(e.value)
 
        setCurrency2(e.value * converterCoefficients[currencyName1]);
    }, [currencyName1]);

    return (
        <div>
            <label>{currencyName1}</label>
            <input onChange={onCurrencyChange} value={currency1}></input>
            <button onClick={swapCurrencies}><-></button>
            <label>{currencyName2}</label>
            <span>{currency2}</span>
        </div>
    );
}




/*
Выравнивание по центру
Выровнять блок .center по вертикали и горизонтали по центру экрана.
<style>
.center {
    border: 1px solid red;
    background: gray;
}
</style>
<body>
    <div class="center">some content</div>
</body>
Можно добавлять обвязки.
*/
```