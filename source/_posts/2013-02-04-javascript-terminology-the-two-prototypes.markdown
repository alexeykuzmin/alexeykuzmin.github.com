---
layout: post
title: "Терминология JavaScript: два прототипа"
date: 2013-02-04 02:35
comments: true
categories: [javascript, russian, translation]
---

*Перевод статьи [Dr. Axel Rauschmayer](http://rauschma.de/)'а “[JavaScript terminology: the two prototypes](http://www.2ality.com/2013/01/two-prototypes.html)”.*

К сожалению, в JavaScript'е термин “прототип” (“prototype”) имеет несколько значений.


## Прототип 1: связь между объектами
Во-первых, существуют прототипы объектов.  
Согласно спецификации ECMAScript, некоторые свойства объектов являются *внутренними*. Внутренние свойства напрямую недоступны из JavaScript'а, их имена пишутся в двойных квадратных скобках. Одно их таких свойств, [[Prototype]], используется для реализации прототипного наследования. Каждый объект содержит в [[Prototype]] ссылку на свой прототип и т.о. наследует все его свойства. В ECMAScript 5 стало возможно получить ссылку на прототип объекта с помощью функции `Object.getPrototypeOf()`:
{% codeblock lang:javascript %}
Object.getPrototypeOf({}) === Object.prototype
// true
{% endcodeblock %}

И возможно создать новый объект, явно указав его прототип, с помощью функции `Object.create()`:
{% codeblock lang:javascript %}
var proto = { foo: 123 };
var obj = Object.create(proto);
obj.foo
// 123
{% endcodeblock %}

Подробнее про обе эти функции можно почитать по ссылке [[1]](#ref1). В ECMAScript 6 можно будет получить ссылку на прототип объекта, используя специальное свойство `__proto__` [[2]](#ref2).


## Прототип 2: свойство конструктора
Во-вторых, каждый конструктор имеет свойство `prototype`.  
Это свойство содержит ссылку на объект, который будет прототипом всех объектов, которые будут созданы с помощью этого конструктора.

{% codeblock lang:javascript %}
function Foo() {}
var f = new Foo();
Object.getPrototypeOf(f) === Foo.prototype
// true
{% endcodeblock %}


## Разрешаем конфликт именований
Обычно из контекста понятно, какой из прототипов имеется ввиду. Если всё-таки возникает неопределённость, то под “prototype” приходится понимать прототип объекта, потому что именно в этом смысле этот термин используется в стандартной библиотеке в названии функции `getPrototypeOf`. Получается, нужно искать другое название для объекта, на который ссылается свойство `prototype` конструкторов. Можно было бы использовать термин “прототип конструктора” (“constructor prototype”), но это не избавит нас от неоднозначности, потому что у конструкторов тоже есть свой прототип:
{% codeblock lang:javascript %}
function Foo() {}
Object.getPrototypeOf(Foo) === Function.prototype
// true
{% endcodeblock %}

В итоге, наилучшим вариантом выглядит “прототип экземпляра” (“instance prototype”).


## Ссылки
<a id="ref1"></a>[1] [JavaScript inheritance by example](http://www.2ality.com/2012/01/js-inheritance-by-example.html)  
<a id="ref2"></a>[2] [JavaScript: \_\_proto\_\_](http://www.2ality.com/2012/10/proto.html)
