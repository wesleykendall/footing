Quick Start
-----------

Listing templates and projects
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Footing manages projects that are started from templates
(specifically, `cookiecutter`_ templates).
In order to see what templates are available for use, do::

    footing ls <forge>

This will list all of the paths of templates that are available under a particular git forge.
A git forge can be:

1. A Github user or organization, such as ``github.com/OrganizationName``
2. A Gitlab group, such as ``gitlab.com/Group`` or ``gitlab.com/Nested/Group``

Doing::

    footing ls <forge> -l

will also display the extended description of the template.

To list all projects created with a template (and the project's descriptions), take the
template path from ``footing ls`` and use it as the second argument like so::

    footing ls <forge> <git@github.com:user/cookiecutter-template-path.git> -l

.. note::

    Be sure to provision a ``GITHUB_API_TOKEN`` or a ``GITLAB_API_TOKEN`` environment
    variable in order for this command to work. The environment variable needs to contain
    a personal access token to the appropriate forge.

.. note::

    This command only works with Gitlab when advanced search is enabled. See
    more `here <https://docs.gitlab.com/ee/user/search/advanced_search.html>`_.

Starting new projects
~~~~~~~~~~~~~~~~~~~~~

A new project can be set up from a template with::

    footing init <template_path>

What happens next is dependent on how the template is configured. By default, `cookiecutter`_ will
prompt the user for template parameters, defined in the ``cookiecutter.json`` file of the template
repository. If any `cookiecutter hooks`_ are defined in the project, additional setup steps will
happen that are specific to the type of project being started.

Keeping your project up to date with the latest template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If a template is ever updated, changes can be pulled into a footing-created project with::

    footing sync

This will git merge the template changes into your repository into a special ``_footing_update`` branch.
You will need to review the changes, resolve
conflicts, and then ``git add`` and ``git push`` these changes yourself.

Sometimes it is desired that projects always remain up to date with the latest template - for example, ensuring
that each project obtains a security patch to a dependency or doing an organization-wide upgrade to a new
version of Python.

Using ``footing sync --check`` from the repository will succeed if the project is up to date with the latest
template or return a non-zero exit code if it isn't. This command can be executed as part of automated testing
that happens in continuous integration in order to ensure all projects remain up to date with changes before
being deployed.

.. note::

	Updating your project with the latest template does not result in `cookiecutter hooks`_ being executed again.

.. note::

    If a ``_footing_update`` branch already exists from a previous update, call ``footing clean`` to delete the
    branch.

Switching your project to another template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sometimes it is desirable to switch a project to another template, like when open sourcing a private package.
Projects can be switched to another template with::

	footing switch <template_path>

Similar to ``footing sync``, you will need to review the changes, resolve conflicts, and then ``git add`` and
``git push`` these changes.

.. note::

	Switching templates does not trigger any `cookiecutter hooks`_. Users must manually do any project setup
	and must similarly do any project teardown that might have resulted from the previously template. The
	authors have intentionally left out this convenience for now since footing currently has no way to spin down projects.


.. _cookiecutter: https://cookiecutter.readthedocs.io/en/latest/
.. _cookiecutter hooks: http://cookiecutter.readthedocs.io/en/latest/advanced/hooks.html
