---
title: "Symfony2 – add legends in a FormType class"
date: 2013-01-06 01:49
comments: true
template: article.jade
categories: Symfony2
author: marcoleong
---

In order to add extra variable in a Symfony 2 form type(in this example, the legend) in FormView, you have to implement this in your form type class.

```php
use Symfony\Component\Form\FormViewInterface;
use Symfony\Component\Form\FormInterface;

//...

public function buildView(FormView $view, FormInterface $form, array $options)
{
    //this add the label, aka legend of the form
    $view->vars = array_replace($view->vars, array(
        'label'        => 'Asset',
    ));
}
```