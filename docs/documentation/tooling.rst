Tooling
=======

Documentation is managed, built and published using:

* `reStructuredText`_ (RST) content stored in GitHub repositories.
* `Sphinx`_ documentation generator.
* `Read the Docs Sphinx Theme`_ for styling.
* `MathJax`_ to render LaTeX equations.
* `Netlify`_ for hosting.

When changes are pushed to the git repository this triggers the Netlify build
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

Obviously projects such as this one (Community Handbook) that are
documentation-only, will have only the Sphinx config and content in their repo.

.. note::
   LaTeX support is currently provided via the ``sphinx.ext.mathjax`` and
   ``sphinx-mathjax-offline`` Sphinx plugins, which render equations client-side
   in the browser. This means that if PDF or ePub downloads are configured,
   any LaTeX equations will be printed literal and not rendered within these. 

Setup
-----

The Python venv module is used to create a virtual environment into which the
Python dependencies are installed. These are specified in requirements.txt, 
which is also used by the Netlify build pipeline. TeX Live is used to convert 
LaTeX to SVG images.

To install git and venv on Ubuntu:

.. tabs:: 

   .. code-tab:: bash
       :caption: Ubuntu 20.04

       sudo apt update

       sudo apt install git python3-venv

   .. code-tab:: bash
       :caption: Ubuntu 22.04+

       sudo add-apt-repository ppa:deadsnakes/ppa

       sudo apt update

       sudo apt install git python3.8 python3.8-venv

This only needs to be done once on each system where documentation is edited.

.. note::
   At the time of writing Netlify build images only have up to Python 3.8, so 
   we need to make sure we also use the 3.8 series, hence additional steps are 
   required with more recent Ubuntu versions.

Then if we wanted to edit the Community Handbook documentation, for example:

.. code-block:: bash

    git clone git@github.com:myriadrf/handbook.git

    cd handbook/docs

    python3.8 -m venv venv

    source venv/bin/activate

    pip install -r requirements.txt

The above steps need would to be carried out for each project where we want to
edit documentation.

Use
---

Simply edit the RST content and then to build:

.. code-block:: bash

   make html

Following which the generated HTML can be found in the ``_build`` directory.

With project repositories that contain documentation and other outputs, such as
software sources or gateware etc., please prefix the git commit message with
"DOCS: " so that it is clear that the commit pertains to documentation only.

.. note::
   1. With every new terminal it will be neccessary to source the script
      ``venv/bin/activate`` in order to activate the Python virtual environment.
   2. The pip install command may need to be run from time to time, e.g. after
      a dependency has been updated or a new one has been configured.
   3. If your Python virtual environment somehow gets in a mess, just delete it,
      re-initialise and pip install the dependencies again.

.. warning::
   Do not attempt to commit the contents of the venv directory, as we don't want
   this to be pushed to GitHub. This usually shouldn't be possible, as the venv
   directory should be listed in .gitignore for all projects, but it's worth 
   noting this here also just in case!

.. _reStructuredText: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
.. _Sphinx: https://www.sphinx-doc.org
.. _Read the Docs Sphinx theme: https://sphinx-rtd-theme.readthedocs.io/en/stable/
.. _MathJax: https://www.mathjax.org/
.. _Netlify: https://www.netlify.com/
