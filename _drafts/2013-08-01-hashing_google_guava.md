---
layout: post
title: "Hashing - Google Guava"
tagline: ""
description: "Gerando um Código de Espalhamento mais elaborado"
category: articles
tags: [java, hash]
---

### Hash Code ou Código de Espalhamento

É muito fácil associar um código de espalhamento com o método [`Object.hashCode()`](http://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#hashCode%28%29){:target="_blank"}, principalmente para programadores Java. Porém, a inteção não é criar mais um post para falar sobre [tabela de espalhamento](http://en.wikipedia.org/wiki/Hash_table){:target="_blank"}, [estutura de dados](http://en.wikipedia.org/wiki/Data_structure){:target="_blank"}, [colisão](http://preshing.com/20110504/hash-collision-probabilities){:target="_blank"}, [bucket](http://en.wikipedia.org/wiki/Hash_table#Choosing_a_good_hash_function){:target="_blank"}, etc.

O objetivo é gerar um código de espalhamento que não esteja associado ao método `Object.hashCode()`. Por exemplo: gerar um código de espalhamento para ajudar a identificar uma instância de um objeto ou até mesmo um conjunto de instâncias contidos dentro de uma lista.

### Java 7

A classe [`java.util.Objects`](http://docs.oracle.com/javase/7/docs/api/java/util/Objects.html#hash%28java.lang.Object...%29){:target="_blank"} foi disponibilizada no Java 7 e possui uma série de métodos estáticos para atuar sobre objetos.

O método `Objects.hash(Object...values)` gera um código de espalhamento de acordo com os valores informados. Neste caso, o código de espalhamento será gerado como se todos os valores informados fossem colocados dentro de um `array` e posteriormente esse `array` será enviado como parâmetro para o método `Arrays.hashCode(Object[])`.

{% highlight java %}
  Objects.hash(myObject.getId(), myObject.getName());
{% endhighlight %}

O código de espalhamento gerado pelo método `Object.hashCode()` ou `Objects.hash(Object...values)` esta restrito a 32 bits, pois ambos os métodos retornam `int`. Devido a isso, muitas vezes o código de espalhamento não possui uma implementação adequada.
Para gerar um código de espalhamento mais elaborado, podemos utilizar o [`Google Guava`](http://code.google.com/p/guava-libraries/wiki/HashingExplained){:target="_blank"} como opção.

### Google Guava 

Com o Google Guava é possível ir além de uma implementação limitada a um retorno de 32 bits. Com ele é possível criar uma `HashFunction` que pode implementar uma série de algoritmos, dentre eles: MD5, SHA-1, SHA-256 e SHA-512.

{% highlight java %}
  HashFunction hashFunction = Hashing.md5();
  hashFunction.newHasher()
	      .putInt(myObject.getId())
	      .putString(myObject.getName(), Charsets.UTF_8)
	      .hash();
{% endhighlight %}
















	