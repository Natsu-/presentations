# DjangoCon 2015 lightning recap

---

# Me (Romain Garrigues @rgarrigues)

- Web Developer @ ![iwoca-logo][iwoca_logo]
- Python/Django developer for 5 years
- Maintainer of some little django apps ([django-dirtyfields](https://github.com/smn/django-dirtyfields), [django-deep-collector](https://github.com/iwoca/django-deep-collector/))
- Presentation available @ [https://github.com/romgar/presentations/](https://github.com/romgar/presentations/)

[iwoca_logo]: images/logo_iwoca.png

---

# Overall feeling

- Focused on well being (and burnout/depression witness)
- Friendly community (lot of first-time speakers)
- Need to go outside (meet people, see real talks)

---

# Architecture / Separating concerns

*(@HannaKollo, @codeinthehole)*

- Avoid big views.py files.
- Use service layers to move business logic from views.

This:

    !python
    def my_view(request):
        params = request.GET
        # Lot of code here before returning HttpResponse
        # ...
        return HttpResponse("This is a too big view")

could be:

    !python
    from my_project.services import deal_with_data

    def my_view(request):
        params = request.GET
        deal_with_data(params)
        return HttpResponse("We have separated business from view logic")

---

# Testing

*(@magopian, @RaeKnowler)*

## py.test

- Syntax more pythonic,
- Interesting options (launch only last failed tests)
- pytest-django: *--reuse-db* and *--create-db*

Added value compare to manage test:

- verbatim output when test is failing, (dict assertion will give exact missing or excess element, instead of full dict)
- hide all logging when test are passing by default
- the console output is more readable and intuitive

---

# Testing ... 2

## Hypothesis

Use randomised data and find edge cases for you

    !python
    @given(floats())
    def test_negation_is_self_inverse(x):
        assert x == -(-x)

---

# Django admin

*(@olasitarska)*

- Use for: small tricks to improve UI
- Other themes: django-flat-theme, django-suit (commercial)
- **Don't** use for: end-users UI, big customisations
- **Don't** allow admin to do massive damage to website. (deleting critical entry, etc)

![django admin dashboard with django-flat-theme][admin_dashboard]

[admin_dashboard]: images/admin_dashboard.png

---

# Security

*(@erikpub, @ubernostrum)*

- Test 12 basic security issues: [ponycheckup.com](https://www.ponycheckup.com/)
- Examples of Django protections:
    * Timing attack on password
    * Sanity checks on choice field (can't give unexpected choice to server)
- Check your package versions [requires.io](https://requires.io/)

---

# Rabbit hole

*(@asendecka, @thomaswturner)*

# Definition
- Trying to resolve a problem that takes ages at the end
- Hard to diagnose
- Ask for help, do some breaks

# Example (Django on Desktop)
Unbelievable stack (Django, C++, Firebird, CEF, Sikuli)

---

# Performance issues

*(@piquadrat_ch)*

## Big Three
- Step 1: reduce SQL queries (select_related, prefetch_related on reverse FKey/M2M)
- Step 2: do less work
    * move work out of request/response cycle (celery)
    * only fetch what you need (QuerySet.defer())
    * do calculations on the db (annotate, aggregate)
- Step 3: caching (hard to invalidate, *johnny-cache* or *django-cache-machine*)

## Tools
- *django-debug-toolbar*
- *django-devserver*

---

# Real-time

*(@aaronbassett)*

# Swamp dragon

![swampdragon architecture][swampdragon_architecture]

[swampdragon_architecture]: images/swampdragon_architecture.png

---

# ORM

*(@akaariai)*

- Improving API for lookup, transforms, expressions

## HStoreField (PostGreSQL, Django 1.8+)

    !python
    class Dog(models.Model):
        data = HStoreField()

    Dog.objects.create(data={'breed': 'labrador', 'owner': 'Bob'})
    Dog.objects.filter(data__breed='collie')

## ArrayField (PostGreSQL, Django 1.8+)

    !python
    class Post(models.Model):
        tags = ArrayField(models.CharField(max_length=200), blank=True)

    Post.objects.create(name='First post', tags=['thoughts', 'django', 'd'])
    Post.objects.filter(tags__contains=['django', 'thoughts'])

---

# Useful tools

## [git-crypt](https://github.com/AGWA/git-crypt/)
Encrypt/decrypt files on GitHub

Initialise your repository:

    !shell
    git-crypt init

Create a *.gitattributes* file to encrypt (in this example) *.key* files

    !text
    secretfile filter=git-crypt diff=git-crypt
    *.key filter=git-crypt diff=git-crypt

## [cookiecutter](https://github.com/audreyr/cookiecutter)
Django project templating

## [CAMEL project](http://reinout.vanrees.org/weblog/2015/06/01/05-cardiff.html)
Convert LaTeX documents to data saved in database, and render it through Django.

---

# ![iwoca-logo][iwoca_logo]

- Lend money to small/medium businesses,
- Non middle-age technologies

![iwoca architecture][iwoca_architecture]

[iwoca_architecture]: images/iwoca_architecture.png

---

# Conclusion

- REALLY friendly
- Take care of yourself !!
- Not so technical -> djangounderthehood.com
- Pies in pieminister and welsh cakes were ... waouh.
- Presentation available @ [https://github.com/romgar/presentations/](https://github.com/romgar/presentations/)
