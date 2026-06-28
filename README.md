# Django Template Inheritance

## Introduction

Template Inheritance is one of the most powerful features of Django's template system. It allows you to create a **base template** that contains the common structure of your website (such as the header, navigation bar, sidebar, and footer), while child templates inherit this structure and replace only the parts that are different.

Instead of repeating the same HTML code in every page, you write it once in the base template and reuse it across your entire project.

---

# Why Use Template Inheritance?

Without template inheritance, every HTML page would contain duplicate code.

### Without Template Inheritance

```html
<!DOCTYPE html>
<html>
<head>
    <title>Home</title>
</head>
<body>

<header>
    <h1>My Website</h1>
</header>

<nav>
    Home | About | Contact
</nav>

<h2>Home Page</h2>

<footer>
    Copyright 2026
</footer>

</body>
</html>
```

Now imagine creating 20 pages.

You would need to copy:

- Header
- Navigation
- Footer
- CSS Links
- JavaScript Links

into every page.

If you later change the navigation bar, you'll have to update all 20 pages.

This is inefficient.

---

# Solution: Template Inheritance

Django allows one template to inherit another using:

```django
{% extends "base.html" %}
```

The child template inherits everything from the parent template and only replaces specific sections using **blocks**.

---

# How Template Inheritance Works

```
                 base.html
                     │
      ┌──────────────┼──────────────┐
      │              │              │
 home.html      about.html     contact.html
```

Every page shares the same layout.

Only the page content changes.

---

# Basic Folder Structure

```
project/
│
├── app/
│   ├── views.py
│   ├── urls.py
│   └── templates/
│       │
│       ├── base.html
│       ├── home.html
│       ├── about.html
│       └── contact.html
│
└── manage.py
```

---

# Creating the Base Template

Create a file named:

```
base.html
```

Example:

```html
<!DOCTYPE html>
<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport"
          content="width=device-width, initial-scale=1.0">

    <title>
        {% block title %}
        My Website
        {% endblock %}
    </title>

    <script src="https://cdn.tailwindcss.com"></script>

</head>

<body>

    <header class="bg-blue-600 text-white p-5">
        <h1 class="text-3xl font-bold">
            Django Website
        </h1>
    </header>

    <nav class="bg-gray-800 text-white p-4">

        <a href="/" class="mr-4">Home</a>

        <a href="/about/" class="mr-4">About</a>

        <a href="/contact/">Contact</a>

    </nav>

    <main class="p-8">

        {% block content %}

        Default Content

        {% endblock %}

    </main>

    <footer class="bg-gray-200 p-5 text-center">

        Copyright © 2026

    </footer>

</body>

</html>
```

---

# Understanding Blocks

Blocks define replaceable sections.

Example:

```django
{% block content %}

Default Content

{% endblock %}
```

The child template can replace everything inside this block.

---

# Creating Home Page

```
home.html
```

```html
{% extends "base.html" %}

{% block title %}
Home
{% endblock %}

{% block content %}

<h2 class="text-3xl font-bold">
    Welcome to Home Page
</h2>

<p class="mt-4">

    This page inherits everything from base.html.

</p>

{% endblock %}
```

---

# Creating About Page

```
about.html
```

```html
{% extends "base.html" %}

{% block title %}
About
{% endblock %}

{% block content %}

<h2 class="text-3xl font-bold">
About Us
</h2>

<p class="mt-4">

We build Django websites.

</p>

{% endblock %}
```

---

# Creating Contact Page

```
contact.html
```

```html
{% extends "base.html" %}

{% block title %}
Contact
{% endblock %}

{% block content %}

<h2 class="text-3xl font-bold">
Contact Us
</h2>

<form>

<input type="text"
       placeholder="Your Name">

</form>

{% endblock %}
```

---

# Visual Representation

```
Base Template

--------------------------------------

Header

Navigation

Block Content

Footer

--------------------------------------
```

Home Page

```
Header

Navigation

Welcome Home

Footer
```

About Page

```
Header

Navigation

About Us

Footer
```

Contact Page

```
Header

Navigation

Contact Form

Footer
```

---

# Template Tags Used

## extends

Used to inherit another template.

```django
{% extends "base.html" %}
```

---

## block

Defines a replaceable section.

```django
{% block content %}

{% endblock %}
```

---

## endblock

Marks the end of a block.

```django
{% endblock %}
```

---

# Multiple Blocks Example

```
base.html
```

```html
<title>

{% block title %}
Website
{% endblock %}

</title>

<body>

{% block content %}

{% endblock %}

</body>
```

Child template

```html
{% extends "base.html" %}

{% block title %}

Home

{% endblock %}

{% block content %}

<h1>Hello</h1>

{% endblock %}
```

---

# Including Reusable Components

Instead of writing the navigation multiple times, create:

```
navbar.html
```

```html
<nav>

<a href="/">Home</a>

<a href="/about/">About</a>

<a href="/contact/">Contact</a>

</nav>
```

Then include it.

```
base.html
```

```django
{% include "navbar.html" %}
```

This inserts the entire navbar.

---

# Difference Between extends and include

| Feature | extends | include |
|----------|----------|----------|
| Reuses full page layout | ✅ | ❌ |
| Reuses small components | ❌ | ✅ |
| Parent-child relationship | ✅ | ❌ |
| Used once per template | ✅ | ❌ |

---

# Passing Data From Views

views.py

```python
from django.shortcuts import render

def home(request):

    return render(request,
                  "home.html",
                  {
                      "name": "Ratul"
                  })
```

home.html

```html
{% extends "base.html" %}

{% block content %}

<h1>

Hello {{ name }}

</h1>

{% endblock %}
```

Output

```
Hello Ratul
```

---

# Common Mistakes

## Forgetting extends

Wrong

```html
<h1>Home</h1>
```

Correct

```django
{% extends "base.html" %}
```

---

## Missing endblock

Wrong

```django
{% block content %}
<h1>Hello
```

Correct

```django
{% block content %}

<h1>Hello</h1>

{% endblock %}
```

---

## Block Names Must Match

Base

```django
{% block content %}
{% endblock %}
```

Child

Wrong

```django
{% block body %}
```

Correct

```django
{% block content %}
```

---

# Best Practices

- Create only one base template.
- Keep common HTML in the base template.
- Use meaningful block names.
- Keep child templates simple.
- Use `include` for reusable components like:
  - Navbar
  - Footer
  - Sidebar
  - Cards
  - Alerts
- Avoid duplicating HTML.

---

# Advantages

- Less code duplication
- Easier maintenance
- Consistent design
- Better project organization
- Faster development
- Cleaner templates
- Reusable components
- Easy to update layouts

---

# Real-World Project Structure

```
templates/
│
├── base.html
│
├── includes/
│   ├── navbar.html
│   ├── footer.html
│   └── sidebar.html
│
├── home.html
├── about.html
├── contact.html
├── services.html
├── blog.html
└── profile.html
```

---

# Summary

Template inheritance allows multiple pages to share a common layout while customizing only the sections that change. By using `{% extends %}` and `{% block %}`, you can build clean, reusable, and maintainable Django applications. Combining inheritance with `{% include %}` for reusable components results in a modular project structure that is easier to develop, test, and maintain.
