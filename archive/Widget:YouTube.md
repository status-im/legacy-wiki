<noinclude> This widget allows you to add a [YouTube video
player](https://developers.google.com/youtube/player_parameters) to your
wiki page.

## Using this widget

For information on how to use this widget, see the [widget description
page on MediaWikiWidgets.org](https://www.mediawikiwidgets.org/YouTube).

## Copy to your site

To use this widget on your site, just install the [MediaWiki Widgets
extension](https://www.mediawiki.org/wiki/Extension:Widgets) and the
copy \[ full source code\] of this page to your wiki, as an article
called ****.
</noinclude><includeonly><iframe width="<!--{$width|escape:'html'|default:'425'}-->" height="<!--{$height|escape:'html'|default:355}-->" src="//www.youtube.com/embed/<!--{if isset($playlist)}-->?listType=playlist&list=<!--{$playlist|escape:'urlpathinfo'}--><!--{else}--><!--{$id|escape:'urlpathinfo'}--><!--{/if}-->" frameborder="0" allowfullscreen></iframe></includeonly>