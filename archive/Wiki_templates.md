Templates are standard wiki pages whose content is designed to be
embedded inside other pages.

The simplest use of templates is as follows. If you create a page called
"Template:Welcome" with contents:

    Hello! Welcome to the wiki.

you have created your first template\! If you then insert the code:

{{Welcome}}

in any other page, when that page is viewed the text "Hello\! Welcome to
the wiki." will appear instead of `{{Welcome}}`. The template content is
"transcluded" into the other page, i.e. it is integrated in the page.

You can then insert `{{Welcome}}` at any point of any page where you
wish to welcome someone. Suppose it is used in 100 pages. If you then
change the template contents to: