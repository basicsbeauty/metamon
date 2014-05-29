メタモン (Metamon)
==================

```
      /~\_.-~-._,--.._
     |                ~-.._
      |     .     .        \
      |                    /
      |     --._..'       | \
      /                      \
     |                        |
     |                        \
     /                         |
     \_.-._  __...___.._______/
           ~~
```

Metamon is a Vagrantfile combined with a set of Ansible Playbooks which can be used to quickly start a new Django project.


Features
--------

Metamon will:
* Set-up basic Operating system dependencies.
* Set-up a Virtualenv and automatically install dependencies.
* Set-up Supervisord and PostgreSQL 9.3, Gunicorn and Nginx.
* Start a new Django project if it's needed.
* Automatically activate a virtualenv and `cd` to the project's directory when logging in during development.
* Use separate requirements files for faster deploys.
* Separate settings file for unit testing with coverage and customized settings to make testing faster.

Installation
------------
1. Donwload and install [Virtualbox](https://www.virtualbox.org/wiki/Downloads).
2. Download and install [Vagrant](https://www.vagrantup.com/downloads.html).
3. Install [Ansible](http://www.ansible.com/home).
   1. Clone the [Ansible Repository](https://github.com/ansible/ansible).
   2. Do `sudo make` and `sudo make install`.
   3. Make sure you have all dependencies installed, run `pip install jinja2 PyYAML`.
4. Copy the Metamon files to your project's root directory, customize, and run. If you already have your code in there, no project should be created.


Configuration and Customization
-------------------------------
### Configuration
There are two values on `all.yml` that need to be set before the Playbooks can be run. `project_name` needs to be given the project's name. It will be used for finding the directory containing the Django project (or to create it) and used for pointing to some of the modules (for example urls). `secret_key` needs to also be set and is used in Django's settings file in `SECRET_KEY`.

#### Vagrant
By default, Vagrant will provide a machine called `dev` that can be reached at `192.168.50.10`. Several ports are forwarded:
* `80` to `8080` (for Nginx).
* `8000` to `8000`.
* `9000` to `9000` (for Gunicorn).
All of this can be changed by editing `Vagrantfile`.

#### Requirements
The requirements for the Django application will also be installed automatically, however, they are split into three different files. There are also settings that define which requirement files are used during installation. All requirements files are under the `/deploy/roles/virtualenv/files/` directory.
* `requirements.txt` should hold the packages needed to run the Django application. Installation is marked by `install_application_requirements` and it automatically set for all hosts. It's probbaly a bad idea to set it to `No`.
* `test_requirements.txt` should hold packages needed for running unit tests but not required by the application. installation is marked by `install_testing_requirements` and it is automatically set to `Yes` on vagrant only.
* `dev_requirements.txt` should hold packages needed only when developing (ipdb for example). Installation is marked by `install_development_requirements` and it is automatically set to `Yes` on vagrant only.

#### Settings and settings for testing
Settings are automatically generated by the `/deploy/roles/application/templates/django/settings*.py.j2` files. Two settings files are generated, one for the regular Django settings and one for running the unit tests. You probably want to run tests like so:

`python manage.py test --settings=project_name.settings_test`


TODO
----

- [x] Support Ubuntu 14.04.
- [x] Refactor variables by moving them to their corresponding roles.
- [ ] Create a script to run tests based on a template.
- [ ] Add a small wizard that can customize `Vagrantfile` settings as well as `project_name` and `secret_key`.
- [ ] Pull repository instead of creating the project (or adding it manually).
- [ ] ZSH for development.
- [ ] Optionally create a sphinx project for documentation.
