---
title: "Composer - Speed up PHP Composer install with different protocols"
date: 2013-01-06
comments: true
categories: Symfony2
template: article.jade
author: marcoleong
---

If you found composer is slow at cloning, you can add this to composer.json

```json
// add to config section composer.json
"config": {
    "bin-dir": "bin",
    "process-timeout": "10000",
    "github-protocols": ["https"]
},
```

This would change protocol use to clone from git (by default) to https which is much faster.
