Aiven Developer
===============

This is the source for the Aiven developer documentation at https://developer.aiven.io. We are working to move all of our technical documentation to this platform, and welcome issues and pull requests from everyone.

It is Python-based, with content in `ReStructuredText (rst) <https://docutils.sourceforge.io/rst.html>`_ and rendered by `Sphinx <https://www.sphinx-doc.org/en/master/>`_.

Contributing
------------

Check the `CONTRIBUTING guide <CONTRIBUTING.rst>`_ for details on how to create content for Aiven Developer. You will find the style guide, pull request process and templates for the various content types there.

Local Development
-----------------

We recommend using a virtual environment like `venv <https://docs.python.org/3/library/venv.html>`_::

    python3 -m venv /path/to/new/virtual/environment

If you call it something other than `venv` or `env`, make sure to add your preferred name to `<.gitignore>`_.

Activate your virtual environment using the `activate` script for your environment.

Install the dependencies::

    pip install -r requirements.txt

Start the HTML version with::

    make livehtml

Your preview should be at http://localhost:8000 (if anything doesn't seem to re-render well, the navigation seems particularly unreliable, try ``make clean`` and then ``make livehtml`` again).

> If you are working on the templates, try the additional `make livehtmlall` command. This disables sphinx's incremental build and also observes changes to the assets, so it's slower but more like a full rebuild when things change.

Windows users
'''''''''''''

Replace all `make` commands with `./make` - this uses the `make.bat` file instead of `Makefile` but the functionality should be equivalent.

Running build tasks locally
'''''''''''''''''''''''''''

To run the spell check locally, you will need to have `Vale <https://github.com/errata-ai/vale>`_ installed on your computer and available on your path.

* Check links: ``make linkcheck``
* Check spelling: ``make spell``

If the spellchecker is rejecting words that are valid (such as technology terms), double check the spelling and capitalisation, then add the word to ``.github/styles/Vocab/Docs/accept.txt``.

Navigation structure
''''''''''''''''''''

The left-hand navigation menu is driven by a plugin called `Sphinx external TOC <https://sphinx-external-toc.readthedocs.io/en/latest/intro.html>`_. You can find our structure in ``_toc.yml``.

Formatting tips
---------------

Here's an incomplete but helpful collection of tips for formatting your content on Aiven Developer.

Links
'''''

Links are different depending on whether they are external links, links pointing to a specific page on the site, or links pointing to a specific anchor or label.

External links are used for external hyperlinks::

    `ReStructuredText <https://docutils.sourceforge.io/rst.html>`_

If you get the warning ``Duplicate target name``, because of multiple links with the same label, see :doc:`Create anonymous links <docs/documentation/anonymus-links>`.

To link to another page on the site, use the `:doc: <https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html#cross-referencing-documents>`_ role::

    Use the :doc:`cli` for scriptable, repeatable actions with Aiven


The ``:doc:`` role uses the page title but if you want to change the link text, you can do so::

    With an :doc:`API <api/index>` you can build any integration you choose

To create a label to link to a particular section (this is also useful if renaming titles that might have links pointing to the old wording), place the label immediately before the section heading::

    .. _tools_cli_tips_tricks:

    Tips and Tricks
    ===============

Then you can refer to the label with a `:ref: <https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html#cross-referencing-arbitrary-locations>`_ entry::

    There are some :ref:`_tools_cli_tips_tricks` to assist you.


Diagrams
''''''''

Diagrams use `sphinxcontrib-mermaid <https://github.com/mgaitan/sphinxcontrib-mermaid>`_ and `mermaid.js <https://mermaid-js.github.io/mermaid/#/>`_ syntax.


Sections and panels
'''''''''''''''''''

We're using `sphinx-panels <https://sphinx-panels.readthedocs.io>`_ for the card layout you see on the homepage and in a few other places. Please use sparingly :)

Code samples
''''''''''''

Code samples should be autodetected (using `pygments <https://pygments.org/>`_) and also will automatically include the "click to copy" button in the top right thanks to `sphinx-copybutton <https://sphinx-copybutton.readthedocs.io>`_.

Do not include a `$` before a command that the user should run, because it will get copied into the user's clipboard and cause the command to fail (this has been a common standard in the past).

Importing content
-----------------

Some of the content for DevPortal came from a previous incarnation of documentation. There is an import script to help with this process.

To set up the import tooling for the first time:

* Install `pandoc <https://pandoc.org/>`_ and make sure the command is in your path
* Change into the ``utils/`` directory
* Run ``pip install -r requirements.txt``

To bring in a page from the previous platform:

* Run ``python import-help-article.py [paste a URL]``
* Take the resulting ``*.rst`` file and any images, and place them as appropriate in the file structure of the project

Migration status
-----------------

You can check the migration status from ``https://help.aiven.io/en`` articles to the ``https://developer.aiven.io/`` articles by using the ``page_stats.py`` script available in this repository. 

Install the dependencies::

    pip install -r requirements-dev.txt

To run the ``page_status.py`` script::

    python page_stats.py    

License
-------

This work is licensed under a
`Creative Commons Attribution 4.0 International License <http://creativecommons.org/licenses/by/4.0/>`_.
