Tooling
=======

Documentation is authored, built and published using:

* `reStructuredText`_ (RST) content stored in GitHub repositories.
* `Sphinx`_ documentation generator.
* `Read the Docs Sphinx Theme`_ for styling.
* `MathJax`_ to render LaTeX equations.
* `Node.js`_ to provide a local webserver and live-reload during editing.
* `Cloudflare`_ for building and hosting HTML.

When changes are pushed to a git repository this triggers the Cloudflare build
pipeline and the resulting HTML is hosted via their CDN. However, before this is
done the site should be generated locally, so as to verify formatting and avoid
continually pushing, having to wait for the site to build and risking errors
going live. Only when the local build looks good do we commit and push.

RST content is located alongside project materials — e.g. software sources and
PCB design databases — in the same git repository. This has a number of
benefits:

* The same workflow with branching and pull requests etc. can be used to manage
  contributions.
* Sphinx can auto-document Python code and via plugins, other programming
  languages that are supported by Doxygen.  
* Project materials + documentation are managed and tagged together.

.. note::
   At the present time documentation for Altium PCB projects is kept on a dedicated *docs* branch. This is so that large binary PCB files do not have to be cloned when simply editing or building the documentation. This may change in future if we move to using LFS to store the Altium files.

Projects such as this one (Community Handbook) that are documentation-only, will have only the Sphinx config and content in their repo.

The same tooling can be used to generate PDF and eBook versions of the 
documentation and this just requires extra build dependencies to be installed.

.. note::
   HTML LaTeX support is currently provided via the ``sphinx.ext.mathjax`` and
   ``sphinx-mathjax-offline`` Sphinx plugins, which render equations client-side
   in the browser. This means that if PDF or ePub files are generated any LaTeX equations will be printed literal and not rendered within these. 

.. note:: 
   PDF and eBook styling has not been configured yet and and the CI build pipelines have not been set up to generate these formats.

Setup
-----

Basic
^^^^^

The following basic setup steps should be completed regardless of whether you
want to build HTML, PDF, eBook documentation or all three.

The Python *venv* module is used to create a virtual environment into which the Python dependencies specified in *requirements.txt* are installed. 

Node.js is used to provide a local webserver and browser live-reload funcionality during editing.

To install the O/S dependencies on Ubuntu:

.. code-block:: bash

   cd ~

   curl -sL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh

   sudo bash nodesource_setup.sh

   sudo apt update

   sudo apt install git python3.13 python3.13-venv nodejs

This only needs to be done once on each system where documentation is edited.

Then if we wanted to edit the Community Handbook documentation, for example:

.. code-block:: bash

   git clone git@github.com:myriadrf/handbook.git

   cd handbook/docs

   npm install

   npm run init

   npm run dev

The *npm install* command installs the Node.js dependencies for convenient editing. This only needs to be run once.

The *npm run init* command creates the Python virtual environment and installs the dependencies for building documentation. It need only be run once. 

The *npm run dev* command builds the documentation, starts a local webserver,  opens the docs in a browser window and then watches for changes, which trigger the documentation to be rebuilt and browser reloaded. This must be run each time editing is to be done.

The generated documentation can be found in:

* ``_build/html``

PDF/eBook
^^^^^^^^^

The following additional setup steps should be completed if you wish to build PDF and/or eBook documentation. This assumes that *npm run init* has already been run, or the Python virtual environment has otherwise been created and dependencies installed.

.. code-block:: bash

   sudo apt install fonts-freefont-otf latexmk texlive-fonts-recommended texlive-latex-recommended texlive-latex-extra texlive-lang-greek \ 
   tex-gyre texlive-xetex

To build PDF documentation:

.. code-block:: bash

   cd docs

   source venv/bin/activate

   make latexpdf

.. note::
   For some reason it can be neccesary to execute the last command twice when
   generating PDFs, possibly as a result of the the way LaTeX build works.

Following which the generated documentation can be found in:

* ``_build/latex`` (PDF)

With project repositories that contain documentation and other outputs, such as
software sources or gateware etc., please prefix the git commit message with
"Docs: " so that it is clear that the commit pertains to documentation only.

Updating
^^^^^^^^

The Python dependencies may be updated from time to time, which might result in local builds failing if your virtual environment is out of date. The cleanest way to resolve this is to delete the existing virtual environment and re-run *npm run init*:

.. code-block:: bash

   cd docs

   rm -rf venv

   npm run init

.. warning::
   Do not attempt to commit the contents of the venv or node_modules directories, as we don't want these to be pushed to GitHub. This usually shouldn't be possible, as the directories should be listed in *.gitignore* for all projects, but it's worth noting this here also just in case.

.. _reStructuredText: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
.. _Sphinx: https://www.sphinx-doc.org
.. _Read the Docs Sphinx theme: https://sphinx-rtd-theme.readthedocs.io/en/stable/
.. _MathJax: https://www.mathjax.org/
.. _Node.js: https://nodejs.org/
.. _Cloudflare: https://cloudflare.com/
