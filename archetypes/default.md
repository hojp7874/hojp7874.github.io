---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
hero: 
description:
menu:
  sidebar:
    name: {{ replace .Name "-" " " | title }}
    identifier: {{ replace .Name "-" " " | title }}
    parent: 
    weight: 10
draft: false
---

