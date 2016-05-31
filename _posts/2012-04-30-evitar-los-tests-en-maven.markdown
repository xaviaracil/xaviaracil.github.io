---
author: desarrolloenmac
comments: true
date: 2012-04-30 14:17:30+00:00
layout: single
slug: evitar-los-tests-en-maven
title: Evitar los tests en maven
wordpress_id: 214
categories:
- Java
tags:
- Java
- maven
---

De acuerdo, SIEMPRE deberían realizarse los test, pero cuando estás compilando algo que no es tuyo y los tests fallan, la forma de evitar que maven los ejecuten es mediante el flag





-Dmaven.test.skip=true





Parece una tontería, pero siempre me equivoco al poner este flag
