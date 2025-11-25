Formatting
==========

For general guidance see the `reStructuredText Primer`_ and other online 
resources.

Details are provided below for MyriadRF conventions.

Sections
--------

Every Sphinx document has multiple sections or heading levels. Section headers 
are created by underlining (and optionally overlining) the section title with 
a punctuation character, at least as long as the text.

Levels are not assigned to specific characters and instead the structure is
determined from their succession. However, for the sake of consistency across
projects they should be used in this order:

* ``#`` for title -- with overline, for parts
* ``*`` for subtitle -- with overline, for chapters
* ``=``, for sections
* ``-``, for subsections
* ``^``, for subsubsections
* ``"``, for paragraphs

This follows the Python community best practice.

Levels give structure to the document, which is used in navigation and in the
display in all output formats. 

Regular documents start with a title heading underlined by ``#``. Subtitles are 
optional. Headings are then marked with ``=`` and ``-`` etc.

Links
-----

Links can be defined which are one of:

1. Internal links to other objects within the same Sphinx project (cross-references).
2. *Intersphinx* links to objects in other Sphinx projects, hosted on MyriadRF or elsewhere.
3. External links to websites which are not Sphinx projects.

It is important to use the correct type of link for the context, as this ensures
that navigation and cross-referencing works correctly, and simplifies maintenance should URLs change. In short, use: internal links where possible, intersphinx links where the target is another Sphinx project, and external links only when neither of the first two options are possible.

Internal
^^^^^^^^

Internal cross-references can be created using the various Sphinx roles, such as
``:ref:`` and ``:doc:`` etc. See the Sphinx documentation for more details.

.. warning::
   By directly referencing documents using the ``:doc:`` role, if the target document is subsequently moved or renamed, the link will break and will need to be updated manually.

Intersphinx
^^^^^^^^^^^

Intersphinx works by using a key to reference a remote Sphinx *objects.inv* inventory file, which contains details of all the documents and sections in the target project, and is used at build time to render links. 

The file ``assets/sphinx/intersphinx.json`` in the `static-assets repository`_ is used to maintain a global intersphinx map. Entries for internal projects map to their MyriadRF project slug, while external projects map to their full URL. Abbreviated example:

.. code-block:: json

   {
       "internal": {
           "quickstart": "quick-start",
           "lms8comp": "lms8001-companion"
       },
       "external": {
           "sphinx": "https://www.sphinx-doc.org/en/master/",
           "lc": "https://librecellular.org/"
       }
   }

The ``intersphinx.json`` file is retrieved at build time and for consistency enforces the use of the same intersphinx keys across all projects. 

However, so as to minimise build time, only projects which are actually referenced in the current documentation are included in the final intersphinx mapping. This is configured via two lists in the project configuration file, ``docs/project.py``, where the keys used are specified. 

.. code-block:: python

   intersphinx_internal = [
      'quickstart',
      'lms8comp',
   ]

   intersphinx_external = [
      'lc',
   ]

These should be configured on a case-by-case basis, so that only projects which are relevant to the current documentation are included.

If a mapping is missing from ``intersphinx.json``, not enabled in ``docs/project.py``, or the remote inventory cannot be reached at build time, Sphinx will emit a warning and the associated links will not be rendered, but the build will still complete.

If an external project documentation URL changes, only the ``intersphinx.json`` file need be updated.

.. warning::
   1. Take great care when updating entries in ``intersphinx.json``, as errors here may break the build for multiple projects.
   2. When adding new entries to ``intersphinx.json``, ensure that the target project actually has an *objects.inv* inventory file available at the standard location (i.e. at the root of the built documentation). If not, intersphinx links to that project will not work.
   3. New keys should be short but meaningful, and use lowercase letters and numbers only, with no spaces or punctuation.

The *external* role + *project key* + *reference type* + *target* are then used to create intersphinx links. For example:

.. code-block:: rest

    Please see :external+sdrgw:ref:`FPGA gateware for the LimeSDR family of boards <index:introduction>`.

Would make the text *FPGA gateware for the LimeSDR family of boards* a link to the introduction section of the *sdrgw* index document.

.. warning::
   If documents in other projects are directly referenced using intersphinx and the ``:doc:`` role, and the target document is subsequently moved or renamed, the link will break and will need to be updated manually.   

External
^^^^^^^^

There are multiple places where external links can be defined: at the document, project and global levels.

.. note::
   Before defining a new external link, check whether it has already been defined elsewhere!

1. At the document level.

Here it is possible to define links which are only used within the current document. While they can either be defined inline or at the bottom of the document, for consistency and ease of maintenance they should always be defined at the bottom. E.g.:

.. code-block:: rest

   See the `FDTI website`_ for more details.

   More text....

   .. _FDTI website: https://www.ftdichip.com/

2. At the project level.

Here links can be defined which may be used anywhere within the current Sphinx project. These are defined in the project ``docs/extlinks.conf`` using the same format as for document-level links. E.g.:   

.. code-block:: rest

   .. _FDTI website: https://www.ftdichip.com/

This file is then effectively appended to every document when Sphinx builds the project, hence the links can be used anywhere within it.

3. Globally for all Sphinx projects.

Where a link is likely to be used in multiple Sphinx projects, it can be defined via the ``assets/sphinx/extlinks.conf`` file in the `static-assets repository`_. This uses the same format as the project-level *extlinks.conf* file and as with this, it is appended to every document.

.. warning::
   Care should be taken when defining global links, to ensure that they are indeed likely to be used in multiple projects, otherwise it just adds unnecessary bloat and may increase build times. Furthermore, errors here may break the build for multiple projects.

Subscript and Superscript
-------------------------

Subscript and superscript characters can be generated using the ``:sub:`` and
``:sup:`` roles. For example:

.. code-block:: rest

    Upper limit 10\ :sup:`6`. See note\ :sub:`2`.

Gives:

Upper limit 10\ :sup:`6`. See note\ :sub:`2`.

.. note::
   Interpreted text needs to be surrounded by whitespace or punctuation, which
   means that if we don't want a gap, e.g. between ``10`` and ``6``, or between
   ``note`` and ``2``, we need to escape the whitespace with a backslash.

Non-ASCII Characters
--------------------

Non-ASCII characters can be inserted by using substitutions. For example, the right arrow (|rarr|) is inserted by entering ``|rarr|``.

Substitutions are defined in two places:

1. At the project level.

Substitutions for use anywhere within the current Sphinx project only are configured via the file ``docs/substitutions.conf``. For example, to be able to insert a cat emoji using ``|cat|``, we would add the following line:

.. code-block:: rest

   .. |cat| unicode:: U+1F408

Where possible a standard name should be used, such as the HTML entity name or Unicode character name.

2. Globally for all Sphinx projects.

Where a substitution is likely to be used in multiple Sphinx projects, it can be defined via the ``assets/sphinx/substitutions.conf`` file in the `static-assets repository`_. This uses the same format as the project-level ``substitutions.conf`` file and as with this, it is appended to every document.

.. note::
   In general it is preferable to update the global substitutions, so as to avoid duplication and inconsistencies between projects. 

Equations (LaTeX)
-----------------

For inline math use this format:

.. code-block:: rest

    Here is an equation: :math:`a^2 + b^2 = c^2`.

Which results in:

Here is an equation: :math:`a^2 + b^2 = c^2`.

Alternatively, for a block with multiple equations, which should be separated by
a line:

.. code-block:: rest

    .. math::

       (a + b)^2 = a^2 + 2ab + b^2

       (a - b)^2 = a^2 - 2ab + b^2

Which gives:

.. math::

   (a + b)^2 = a^2 + 2ab + b^2

   (a - b)^2 = a^2 - 2ab + b^2

For further details see the Sphinx `math directive`_.




.. _reStructuredText Primer: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
.. _Math directive: https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#math
.. _static-assets repository: https://github.com/myriadrf/static-assets