---
title: DOM 中 Property 和 Attribute 的区别
---

# DOM 中 Property 和 Attribute 的区别

property 和 attribute非常容易混淆，两个单词的中文翻译也都非常相近（property：属性，attribute：特性），但实际上，二者是不同的东西，属于不同的范畴。

property是DOM中的属性，是JavaScript里的对象；
attribute是HTML标签上的特性，它的值只能够是字符串；

property和attribute之间的区别和联系难以用简单的技术特性来描述，我在StackFlow上找到如下的回答，或者会更加接近于真正的答案：

These words existed way before Computer Science came around.

Attribute is a quality or object that we attribute to someone or something. For example, the scepter is an attribute of power and statehood.

Property is a quality that exists without any attribution. For example, clay has adhesive qualities; or, one of the properties of metals is electrical conductivity. Properties demonstrate themselves though physical phenomena without the need attribute them to someone or something. By the same token, saying that someone has masculine attributes is self-evident. In effect, you could say that a property is owned by someone or something.

To be fair though, in Computer Science these two words, at least for the most part, can be used interchangeably - but then again programmers usually don't hold degrees in English Literature and do not write or care much about grammar books :).

最关键的两句话：

attribute（特性），是我们赋予某个事物的特质或对象。
property（属性），是早已存在的不需要外界赋予的特质。

## 参考
What is the difference between attribute and property?
javascript中attribute和property的区别详解