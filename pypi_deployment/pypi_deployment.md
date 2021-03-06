# PyPI deployment in 5 minutes with ![github-logo][github_logo] ![travis-logo][travis_logo]

[github_logo]: images/github_logo.png
[travis_logo]: images/travis_logo.png

---

# Me (Romain Garrigues @rgarrigues)

- Web Developer @ ![iwoca-logo][iwoca_logo]
- Python/Django developer for 5 years
- Maintainer of some little django apps ([django-dirtyfields](https://github.com/smn/django-dirtyfields), [django-deep-collector](https://github.com/iwoca/django-deep-collector/))
- Presentation available @ [https://github.com/romgar/presentations/](https://github.com/romgar/presentations/)

[iwoca_logo]: images/logo_iwoca.png

---

# The goal

x Be able to install your_lovely_package with pip

    !bash
    $ pip install your_lovely_package

x Easily deploy a new release on PyPI, for example by pushing a tag on master branch.

---

# GitHub: Your project

![github-defaultproject][default_structure]
[default_structure]: images/default_structure.png

You only need:

- Your django app (python package)
- README to describe your project
- setup.py to describe your python package

---

# GitHub: Setup file

- Reusable apps tutorial @ [https://docs.djangoproject.com/en/1.8/intro/reusable-apps/](https://docs.djangoproject.com/en/1.8/intro/reusable-apps/)

Example:

    !python
    import os
    from setuptools import setup

    setup(
        name='your-lovely-package',
        version='0.1',
        packages=['your_lovely_package'],
        include_package_data=True,
        license='BSD License',  # example license
        description='A simple lovely package.',
        long_description='You could read README file and put it there',
        url='https://github.com/romgar/your-lovely-package',
        author='Romain Garrigues',
        author_email='romain.garrigues.cs@gmail.com',
        classifiers=[
            'Framework :: Django',
        ],
    )

---

# Travis CI : Create an account

- Continuous integration platform for GitHub projects @ [https://travis-ci.org/](https://travis-ci.org/)
- Trigger scripts on every commit on every branch of your GitHub projects

![travis-landing_page][travis_landing_page]
[travis_landing_page]: images/travis_landing_page.png

---

# Travis CI: Link GitHub account

![github-login][github_login]
[github_login]: images/github_login.png

---

# Travis CI: Activate GitHub repositories
[https://travis-ci.org/profile/romgar](https://travis-ci.org/profile/romgar)

![travis-activate_repo][travis_activate_repo]
[travis_activate_repo]: images/travis_activate_repo.png

---

# GitHub: configure travis

Create a *.travis.yml* file on your GitHub repository root folder:

    !shell
    language: python

    python:
      - "2.7"

    script:
      - touch foo

---

# GitHub: deploy section in Travis CI config

    !shell
    language: python

    python:
        - "2.7"

    script:
        - touch foo

    deploy:
        provider: pypi
        user: romgar   <--- your PyPI username
        password:
            secure: my_secure_password <--- to be generated
        on:
            tags: true
            branch: master

---

# GitHub: generate secure password

    !shell
    $ gem install travis
    $ travis encrypt --add deploy.password

The generated password will be automatically added to your *.travis.yml* config file.

---

# Summary

- Create a GitHub repository with your package, a README and a setup.py file,
- Activate Continuous Integration of this repository on Travis CI,
- Create a *.travis.yml*, with a deploy section,
- Generate secure password,
- Push files, and deploy it automatically on PyPI depending on conditions,
- Well done !!!
- Every step explained in details @ [5minutes.youkidea.com](http://5minutes.youkidea.com/howto-deploy-python-package-on-pypi-with-github-and-travis.html)
