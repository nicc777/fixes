# Templates

HTML with special tagsfor data, loops and conditions

Templates are also extendable

## Where

Templates looks for `templates` directory inside the app directory by default

Also expects a folder inside `templates` with the same name as the `app`

Call the template `my_file.html`

### Better Way

* Is add global templates, in the root of the project add a `templates` folder
* then add it to the `DIRS` in `settings.py`

### Use Render

    from django.shortcuts import render

    return render(request, 'home.html', {context: context})

## Template tags

{% raw %}`{%` and `%}`{% endraw %} that allow you to write python within

Use `{{ var_name }}` to print out a variable

## Inheriting

* Extending a parent template allows overridable `blocks`
* Name a block {% raw %}`<title>{% block title %}{% endblock %}</title>`{% endraw %}
* Extends: {% raw %}`{% extends "layout.html" %}`{% endraw %}
* Then set block content

## URLs

### Named Url

Give your url's in `urls.py` a name

        url(r'^$', views.courselist, name='list')

Setting parameters

        {% raw %}<h3><a href="{% url 'step' course_pk=step.course.pk step_pk=step.pk %}">{{ step.title}}</a></h3>{% endraw %}

## Filters

        {% raw %}{{ myVar | filter}{% endraw %}

* `linebreaks` - converting linebreaks into valid html
* `join` - joins a list with a between

        myArr|join:", "

* `length` - get length (sometimes better to use built in methods)
* `pluralize` - adds a `s` to a certain word

        step{{ course.set_set.count| pluralize}}
* `wordcount` - counts number of words
* `unordered_list` - no closing tagged, unordered list
* `apnumber` - add associated press style numbers
* `truncatewords:n` - cuts off after that many words and adds an ellipsis
* `date:format` - eg `date:"F j, Y"` for human readable dates
* `urlize` - Converts an email or web address into a link
* `safe` - output raw html - tell django it is safe

Can also be done in other filters with `marksafe`

In settings add to INSTALLED_APPS

        'django.contrib.humanize',

at top of template load humanize

        {% raw %}{% load humanize %}{% endraw %}

Good use case for custom filters:

* Convert markdown to html

### Arguments to custom tags

Arguments are seperated by a space

        {% raw %}
        {% is_conjugate test_var %}
        {% endraw %}

### Load multiple filters

        {% raw %}
        {% load humanize course_extras %}
        {% endraw %}

### Chaining

You can chain filters

        {% raw %}{{ word|lower|capfirst }}{% endraw %}

Applied **in order**, applied to result of one before

## With

Shorten a long variable

        {% raw %}
        {% with con=step.content %}
                {{ con|linebreaks }}
        {% endwith %}
        {% endraw %}

## If

Conditionally show a template

        {% raw %}
        {% if course.description %}
            {{ course.description }}
        {% endif %}
        {% endraw %}

Can also use an else

        {% raw %}
        {% if course.description|wordcountt <= 5 %}
            {{ course.description }}
        {% else %}
            {{ course.description|truncatewords:5 }}
        {% endif %}
        {% endraw %}

## Static files

Remember to let django know you will be using `static` with:

        {% raw %}
        {% load static from staticfiles %}
        {% endraw %}

## Custom template tags

Requires a folder called `templatetags` in the `app`

Add the `__init__.py` file to recognise as a python package

Follow this [Custom Template tag Guide](https://docs.djangoproject.com/en/1.11/howto/custom-template-tags/)

This makes the code more readable and DRY

So important to use these

### Inclusion tag

Different from a simple tag as it includes data as a whole other template

These are powerful

##### Project wide static files

best location: `project/assets/css/*.css`

#### App specific static files

`project/app_name/static/app_name/css/*.css`
`project/app_name/static/app_name/images/*.jpg`
`project/app_name/static/app_name/js/*.js`

App specific css should not be used on pages not within the app itself