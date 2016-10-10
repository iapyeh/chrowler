Syntax Notes
###################

Just for reference, not part of the document of the Chrowler.

Level 1, A
=========

content of level 1

    inner block 1st line
    inner block 2nd line


Level 1, B
===============

number lists

1. first
3. third
4. fourth
2. second

 lists

* item A

* item B

* first
  second line of this item

* second


Level 1, C
========

content of level 1-c

Level 2, C
-----------

content of level 2-c

Level 2, C
--------------

content of level 2-c

Level 2, C
---------------

content of level 2-c

Image
=====

.. image:: static/green.png

Click on my |ImageLink|_

.. |ImageLink| image:: static/green.png
.. _ImageLink: http://www.google.com


Code
====

a block of code::

    class MarkdownReader(BaseReader):
        enabled = bool(Markdown)

        def read(self, source_path):
            """Parse content and metadata of markdown files"""
            text = pelican_open(source_path)
            md_extensions = {'markdown.extensions.meta': {},
                             'markdown.extensions.codehilite': {}}
            md = Markdown(extensions=md_extensions.keys(),
                          extension_configs=md_extensions)
            content = md.convert(text)

            metadata = {}
            for name, value in md.Meta.items():
                name = name.lower()
                meta = self.process_metadata(name, value[0])
                metadata[name] = meta
            return content, metadata

inline code block
------------------

    If your new reader requires additional Python dependencies, then you should wrap
    their ``import`` statements in a ``try...except`` block.  Then inside the reader's
    class, set the ``enabled`` class attribute to mark import success or failure.
    This makes it possible for users to continue using their favourite markup method
    without needing to install modules for formats they don't use.

inline link
------------

    You can download this image :download:`here <_static/green.png>`.

table
======

ere is the list of currently implemented signals:

=================================   ============================   ===========================================================================
Signal                              Arguments                       Description
=================================   ============================   ===========================================================================
initialized                         pelican object
finalized                           pelican object                 invoked after all the generators are executed and just before pelican exits
                                                                   useful for custom post processing actions, such as:
                                                                   - minifying js/css assets.
                                                                   - notify/ping search engines with an updated sitemap.
generator_init                      generator                      invoked in the Generator.__init__
all_generators_finalized            generators                     invoked after all the generators are executed and before writing output
readers_init                        readers                        invoked in the Readers.__init__
article_generator_context           article_generator, metadata
article_generator_preread           article_generator              invoked before a article is read in ArticlesGenerator.generate_context;
                                                                   use if code needs to do something before every article is parsed
article_generator_init              article_generator              invoked in the ArticlesGenerator.__init__
article_generator_pretaxonomy       article_generator              invoked before categories and tags lists are created
                                                                   useful when e.g. modifying the list of articles to be generated
                                                                   so that removed articles are not leaked in categories or tags
article_generator_finalized         article_generator              invoked at the end of ArticlesGenerator.generate_context
article_generator_write_article     article_generator, content     invoked before writing each article, the article is passed as content
=================================   ============================   ===========================================================================

.. warning::

   Avoid ``content_object_init`` signal if you intend to read ``summary``
   or ``content`` properties of the content object. That combination can
   result in unresolved links when :ref:`ref-linking-to-internal-content`
   (see `bug #314`_). Use ``_summary`` and ``_content``
   properties instead, or, alternatively, run your plugin at a later
   stage (e.g. ``all_generators_finalized``).

.. note::

   After Pelican 3.2, signal names were standardized.  Older plugins
   may need to be updated to use the new names:

   ==========================  ===========================
   Old name                    New name
   ==========================  ===========================
   article_generate_context    article_generator_context
   article_generate_finalized  article_generator_finalized
   article_generate_preread    article_generator_preread
   pages_generate_context      page_generator_context
   ==========================  ===========================


The second file is the ``static/css/style.css`` CSS stylesheet:

.. code-block:: css

    body {
        font-family : monospace ;
        border : thin solid gray ;
        border-radius : 5px ;
        display : block ;
    }


.. `bug #314`_: http://www.google.com