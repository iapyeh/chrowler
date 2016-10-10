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
2. second
#. item A
#. item B

 lists

*. item A
*. item B
*. first
   second line of this item
*. second


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

.. image:: static/green.png

Click on my |ImageLink|_

.. |ImageLink| image:: static/green.png
.. _ImageLink: http://www.google.com



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

    If your new reader requires additional Python dependencies, then you should wrap
    their ``import`` statements in a ``try...except`` block.  Then inside the reader's
    class, set the ``enabled`` class attribute to mark import success or failure.
    This makes it possible for users to continue using their favourite markup method
    without needing to install modules for formats they don't use.

