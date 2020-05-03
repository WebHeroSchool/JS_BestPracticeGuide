# Подборка Best Practice JavaScript


### **Используйте `const` для объявления переменных; избегайте var.**

>Это гарантирует, что вы не сможете 
переопределять значения, т.к. это может привести 
к ошибкам и к усложнению понимания кода.

		// bad
		var a = 1;
		var b = 2;

		// good
		const a = 1;
		const b = 2;

### **Для создания объекта используйте литеральную нотацию.** 

>Не используйте `new Object()` 
- Используйте `{}` вместо `new Object()`
- Используйте `""` вместо `new String()`
- Используйте `0` вместо `new Number()`
- Используйте `false` вместо `new Boolean()`
- Используйте `[]` вместо `new Array()`
- Используйте `/()/` вместо `new RegExp()`
- Используйте `function (){}` вместо `new Function()`

        // bad
        const item = new Object();

       // good
       const item = {};
 

### **Используйте строгое сравнение `===`** 
>Оператор сравнения `==` всегда преобразует (в соответствующие типы) перед сравнением.Оператор `===` строго сравнивает значение и тип данных.     

    0 == "";        // true
    1 == "1";       // true
    1 == true;      // true

    0 === "";       // false
    1 === "1";      // false
    1 === true;     // false

### **Избегайте использования eval().**

>Функция `eval()` позволяет выполнить команду записанную в строковой переменной, которая передается в качестве параметра в eval.

>Это замедлит программу , а также представляет проблему безопасности.
```
//bad
eval( "сonsole.log('hello!');" );
//good
сonsole.log('hello!');  //Можно просто написать непосредственно код
```
### **Никогда не используйте конструктор функций для создания новых функий.**
>Создание функции в таком духе вычисляет строку подобно eval(), из-за чего открываются уязвимости.
```
// плохо
var add = new Function('a', 'b', 'return a + b');

// всё ещё плохо
var subtract = Function('a', 'b', 'return a - b');
```

### **Всегда используйте `class`. Избегайте прямых манипуляций с `prototype`.**

> Синтаксис `class` является кратким и понятным.

    // bad
	function Queue(contents = []) {
	  this.queue = [...contents];
	}
	Queue.prototype.pop = function () {
	  const value = this.queue[0];
	  this.queue.splice(0, 1);
	  return value;
	};

	// good
	class Queue {
	  constructor(contents = []) {
	    this.queue = [...contents];
	  }
	  pop() {
	    const value = this.queue[0];
	    this.queue.splice(0, 1);
	    return value;
	  }
	}

### **Используйте `extends` для наследования.**

>Это встроенный способ наследовать функциональность прототипа, не нарушая `instanceof`.

    // bad
	const inherits = require('inherits');
	function PeekableQueue(contents) {
	  Queue.apply(this, contents);
	}
	inherits(PeekableQueue, Queue);
	PeekableQueue.prototype.peek = function () {
	  return this.queue[0];
	};

	// good
	class PeekableQueue extends Queue {
	  peek() {
	    return this.queue[0];
	  }
	}

### **При обращении к нескольким свойствам объекта используйте деструктуризацию объекта.**

>Деструктуризация избавляет вас от создания временных переменных для этих свойств.

            // bad
		function getFullName(user) {
		  const firstName = user.firstName;
		  const lastName = user.lastName;

		  return `${firstName} ${lastName}`;
		}

		// good
		function getFullName(user) {
		  const { firstName, lastName } = user;
		  return `${firstName} ${lastName}`;
		}

		// excellent
		function getFullName({ firstName, lastName }) {
		  return `${firstName} ${lastName}`;
		}

### **Когда вам необходимо использовать анонимную функцию (например, при передаче встроенной функции обратного вызова), используйте стрелочную функцию.**

>Таким образом создаётся функция, которая выполняется в контексте `this`, который мы обычно хотим, а также это более короткий синтаксис.

>Почему бы и нет? Если у вас есть довольно сложная функция, вы можете переместить эту логику внутрь её собственного именованного функционального выражения.

    // bad
	[1, 2, 3].map(function (x) {
	  const y = x + 1;
	  return x * y;
	});

	// good
	[1, 2, 3].map((x) => {
	  const y = x + 1;
	  return x * y;
	});
### **Избегайте  автоматического преобразования типов.**

>JavaScript является слабо типизированным. Неявное преобразование может привести к неожиданным результатам.
```
//bad
let x = 5 + "7";  // x.valueOf() is 57,  typeof x is a string

//good
let x = 5 + 7;  // x.valueOf() is 12,  typeof x is a number
```