---
title: "Symfony2 - find out loaded document classNames in DocumentManager"
date: 2013-01-17
comments: true
template: article.jade
categories: symfony2
author: marcoleong
---

Digged into the source code to find out how to list all the documents mapped in a document manager.

```
$classNames = $documentManager //DocumentManager
    ->getConfiguration()
    ->getMetadataDriverImpl()
    ->getAllClassNames();
```

