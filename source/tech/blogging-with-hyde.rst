==================
Blogging with hyde
==================


Introduction
============
After investigating viable options how to start a weblog, 
starting publishing on our good old TWiki_ @ http://www.netfrag.org/twiki/bin/view [2004],
an intermediate trip using `DokuWiki`_ @ http://open.nshare.de/ [2009], we'll try to settle with hyde_ [2011].
Let's see what the future brings...

.. _TWiki: http://twiki.org/
.. _DokuWiki: http://www.dokuwiki.org/
.. _hyde: http://ringce.com/hyde
	

Why reStructuredText?
---------------------

	So if you need something simple, Markdown is probably the best option. But if you're looking at more advanced needs like structuring whole documents and an extensible system that helps you with kinky stuff, ReST is the preferred choice.
	
	-- http://stackoverflow.com/q/259061

reStructuredText_ became our favorite markup language. We want power.

Some arguments:
	- `Markdown versus ReStructuredText <http://stackoverflow.com/questions/34276/markdown-versus-restructuredtext>`_
	- `Compare and contrast the lightweight markup languages (Textile, Markdown, and reStructuredText) <http://stackoverflow.com/questions/659227/compare-and-contrast-the-lightweight-markup-languages-textile-markdown-and-re>`_
	- `How To Write Papers with Restructured Text <http://www.acooke.org/cute/HowToWrite1.html>`_
	- Whole books have been written in reStructuredText_:
		- http://learncodethehardway.org/
		- http://boostpro.com/mplbook/
	- There is also a dedicated hosting service using Sphinx_: http://readthedocs.org/

.. _reStructuredText: http://docutils.sourceforge.net/rst.html
.. _Markdown: http://daringfireball.net/projects/markdown/
.. _Textile: http://www.textism.com/tools/textile/
.. _Sphinx: http://sphinx.pocoo.org

.. seealso::

	- http://superuser.com/questions/209897/text-formatter-tools/209902
	- http://en.wikipedia.org/wiki/Lightweight_markup_language


Why hyde?
---------
After working with Sphinx_ a few years and being fascinated about the authoring workflow (write-render-browse),
we came across Jekyll_ (used by Github_) for the purpose of creating a static weblog site.
However, Jekyll_ is done in Ruby_ while we are in love with Python_.
More important, Jekyll_ doesn't grok reStructuredText_, but only likes Markdown_ or Textile_.
If **you** are keen on starting to blog with Jekyll_, take a look into Octopress_ and Mou_.

Things we looked at:
	- http://www.subspacefield.org/~travis/python_web_authoring_programming.html
	- http://www.subspacefield.org/~travis/python_web_page_generators.html
	- http://www.subspacefield.org/~travis/static_blog_generators.html
	- http://iwantmyname.com/blog/2011/02/list-static-website-generators.html
	- http://vincent.bernat.im/en/blog/2011-new-website-with-hyde.html
	- https://github.com/xfire/growl/
	- http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html

.. _Jekyll: http://jekyllrb.com/
.. _Github: http://github.com
.. _Ruby: http://ruby-lang.org
.. _Python: http://python.org/
.. _Octopress: http://octopress.org/
.. _Mou: http://mouapp.com/


Vendor lock-in?
---------------

Answer: No!

	About pandoc

	If you need to convert files from one markup format into another, pandoc is your swiss-army knife. Need to generate a man page from a markdown file? No problem. LaTeX to Docbook? Sure. HTML to MediaWiki? Yes, that too. Pandoc can read markdown and (subsets of) reStructuredText, textile, HTML, and LaTeX, and it can write plain text, markdown, reStructuredText, HTML, LaTeX, ConTeXt, PDF, RTF, DocBook XML, OpenDocument XML, ODT, GNU Texinfo, MediaWiki markup, textile, groff man pages, Emacs org-mode, EPUB ebooks, and S5 and Slidy HTML slide shows. PDF output (via LaTeX) is also supported with the included markdown2pdf wrapper script.

	-- pandoc_

.. _pandoc: http://johnmacfarlane.net/pandoc/



Hyde
====

Who did it?
-----------

Lakshmi Vyas
- http://twitter.com/#!/lakshmivyas/
- http://ringce.com


Who's using it?
---------------
- http://www.awsum.co/
- http://www.poxd.org/
- http://hgtip.com/
- http://www.adamtindale.com/
- http://vincent.bernat.im/en/blog/
	- https://github.com/vincentbernat/www.luffy.cx
- https://github.com/hyde/hyde/wiki/Hyde-Powered

Resources
---------
- http://ringce.com/blog/2009/introducing_hyde
- http://hyde.github.com/
- https://github.com/hyde/hyde

	
Bootstrapping
=============
- http://merlin.rebrovic.net/hyde-starter-kit/
- https://github.com/auzigog/hyde-bootstrap
- http://twitter.github.com/bootstrap/
- http://twitter.github.com/bootstrap/examples/container-app.html#


More resources
==============
- http://rst.ninjs.org/
