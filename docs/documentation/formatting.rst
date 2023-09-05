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

Non-ASCII characters can be inserted by using substitutions and these are
defined in the file ``substitutions.txt`` in the root of the Sphinx project.
For example, the right arrow (|rarr|) is inserted by entering ``|rarr|``.  If a
character is not currently supported, simply look up the Unicode ID and add it
to substitutions.txt.

Equations (LaTeX)
-----------------

.. note::
   Where it would be the first use of equations in a project, check and ensure
   that the ``sphinxcontrib-jsmath`` and ``sphinx-mathjax-offline`` Python 
   modules are present in ``requirements.txt``, and that the extensions
   ``sphinx.ext.mathjax`` and ``sphinx-mathjax-offline`` are set up in 
   ``conf.py``.

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
