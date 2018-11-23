## Usage

The basic concept of the <ref> tag is that it inserts the text enclosed
by the ref tags as a footnote in a designated section, which you
indicate with the placeholder tag

<references />

. This format cannot be used interchangeably with the older format — you
must pick one or the other.

Additional placeholder tags

<references />

can be inserted in the text, and all <ref> tags up to that point will be
inserted there.

If you forget to include

<references />

in the article, the footnotes will not disappear, but the references
will be displayed at the end of the page.

This page itself uses footnotes, such as the one at the end of this
sentence.\[1\] If you \[ view the source\] of this page by clicking
"Edit this page", you can see a working example of footnotes.
</translate>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p><translate> Wikitext</translate></p></th>
<th><p><translate> Rendering</translate></p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="sourceCode" id="cb1"><pre class="sourceCode xml"><code class="sourceCode xml"><a class="sourceLine" id="cb1-1" data-line-number="1"><span class="kw">&lt;translate&gt;</span></a>
<a class="sourceLine" id="cb1-2" data-line-number="2"><span class="co">&lt;!--T:24--&gt;</span></a>
<a class="sourceLine" id="cb1-3" data-line-number="3">The Sun is pretty big.<span class="kw">&lt;ref&gt;</span>E. Miller, &#39;&#39;The Sun&#39;&#39;, (New York: Academic Press, 2005), 23-5.<span class="kw">&lt;/ref&gt;</span> The Moon, however, is not so big.<span class="kw">&lt;ref&gt;</span>&#39;&#39;R. Smith, &quot;Size of the Moon&quot;, &#39;&#39;Scientific American&#39;&#39;, 46 (April 1978): 44-6.<span class="kw">&lt;/ref&gt;</span></a>
<a class="sourceLine" id="cb1-4" data-line-number="4"></a>
<a class="sourceLine" id="cb1-5" data-line-number="5">==Notes== <span class="co">&lt;!--T:74--&gt;</span></a>
<a class="sourceLine" id="cb1-6" data-line-number="6"><span class="kw">&lt;/translate&gt;</span></a>
<a class="sourceLine" id="cb1-7" data-line-number="7"><span class="kw">&lt;references</span> <span class="kw">/&gt;</span></a></code></pre></div></td>
<td><p><translate> The Sun is pretty big.[2] The Moon, however, is not so big.[3] </translate></p>
<div style="font-weight:bold;font-family:sans-serif;">
<p><translate> Notes</translate></p>
</div>
<references /></td>
</tr>
</tbody>
</table>

<translate>

### Multiple uses of the same footnote

To give a footnote a unique identifier, use **<ref name="name">**. You
can then refer to the same footnote again by using a ref tag with the
same name. The text inside the second tag doesn't matter, because the
text already exists in the first reference. You can either copy the
whole footnote, or you can use a terminated empty ref tag that looks
like this: **\[4\]**. The quotes are not required (but still correct) if
the name contains no embedded spaces. The space before the closing "/"
may be omitted.

In the following example, the same source is cited three times.
</translate>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p><translate> Wikitext</translate></p></th>
<th><p><translate> Result</translate></p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="sourceCode" id="cb1"><pre class="sourceCode xml"><code class="sourceCode xml"><a class="sourceLine" id="cb1-1" data-line-number="1"><span class="kw">&lt;translate&gt;</span></a>
<a class="sourceLine" id="cb1-2" data-line-number="2"><span class="co">&lt;!--T:79--&gt;</span></a>
<a class="sourceLine" id="cb1-3" data-line-number="3">This is an example of multiple references to the same footnote.<span class="kw">&lt;ref</span><span class="ot"> name=</span><span class="st">&quot;multiple&quot;</span><span class="kw">&gt;</span>Remember that when you refer to the same footnote multiple times, the text from the first reference is used.<span class="kw">&lt;/ref&gt;</span></a>
<a class="sourceLine" id="cb1-4" data-line-number="4"></a>
<a class="sourceLine" id="cb1-5" data-line-number="5"><span class="co">&lt;!--T:80--&gt;</span></a>
<a class="sourceLine" id="cb1-6" data-line-number="6">Such references are particularly useful when citing sources, if different statements come from the same source.<span class="kw">&lt;ref</span><span class="ot"> name=</span><span class="st">&quot;multiple&quot;</span> <span class="kw">/&gt;</span> Any reused tag should not contain extra content, that will spawn an error. Only use empty tags in this role.</a>
<a class="sourceLine" id="cb1-7" data-line-number="7"></a>
<a class="sourceLine" id="cb1-8" data-line-number="8"><span class="co">&lt;!--T:81--&gt;</span></a>
<a class="sourceLine" id="cb1-9" data-line-number="9">A concise way to make multiple references is to use empty ref tags, which have a slash at the end. Although this may reduce redundant work, please be aware that if a future editor removes the first reference, this will result in the loss of all references using the empty ref tags.<span class="kw">&lt;ref</span><span class="ot"> name=</span><span class="st">&quot;multiple&quot;</span> <span class="kw">/&gt;</span></a>
<a class="sourceLine" id="cb1-10" data-line-number="10"></a>
<a class="sourceLine" id="cb1-11" data-line-number="11">==Notes== <span class="co">&lt;!--T:82--&gt;</span></a>
<a class="sourceLine" id="cb1-12" data-line-number="12"><span class="kw">&lt;/translate&gt;</span></a>
<a class="sourceLine" id="cb1-13" data-line-number="13"><span class="kw">&lt;references</span> <span class="kw">/&gt;</span></a></code></pre></div></td>
<td><p><translate> This is an example of multiple references to the same footnote.[5]</p>
<p>Such references are particularly useful when citing sources, if different statements come from the same source.[6] Any reused tag should not contain extra content, that will spawn an error. Only use empty tags in this role.</p>
<p>A concise way to make multiple references is to use empty ref tags, which have a slash at the end. Although this may reduce redundant work, please be aware that if a future editor removes the first reference, this will result in the loss of all references using the empty ref tags.</translate>[7]</p>
<div style="font-weight:bold;font-family:sans-serif;">
<p><translate> Notes</translate></p>
</div>
<references /></td>
</tr>
</tbody>
</table>

<translate>

### Merging two texts into a single reference

A typical wikisource issue is, how to merge into one reference texts
split into different pages. This can be done using a
**<ref name="name">** tag for the first part of the reference, and
tagging the following parts into different pages with a tag
**<ref follow="name">**.

Here an example, deriving the text used into previous section:
</translate>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p><translate> Wikitext</translate></p></th>
<th><p><translate> Rendering</translate></p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="sourceCode" id="cb1"><pre class="sourceCode xml"><code class="sourceCode xml"><a class="sourceLine" id="cb1-1" data-line-number="1"><span class="kw">&lt;translate&gt;</span></a>
<a class="sourceLine" id="cb1-2" data-line-number="2"><span class="co">&lt;!--T:35--&gt;</span></a>
<a class="sourceLine" id="cb1-3" data-line-number="3">This is an example of merging multiple texts into the same footnote.<span class="kw">&lt;ref</span><span class="ot"> name=</span><span class="st">&quot;main&quot;</span><span class="kw">&gt;</span>Remember that all the texts will be included into the reference containing the &#39;&#39;name=&#39;&#39; attribute.<span class="kw">&lt;br</span> <span class="kw">/&gt;&lt;/ref&gt;</span></a>
<a class="sourceLine" id="cb1-4" data-line-number="4"></a>
<a class="sourceLine" id="cb1-5" data-line-number="5"><span class="co">&lt;!--T:36--&gt;</span></a>
<a class="sourceLine" id="cb1-6" data-line-number="6"><span class="kw">&lt;ref</span><span class="ot"> follow=</span><span class="st">&quot;main&quot;</span><span class="kw">&gt;</span>Simply include the text to be merged into the first <span class="kw">&lt;ref</span><span class="ot"> name=</span><span class="st">&quot;main&quot;</span><span class="kw">&gt;</span> reference with a tag <span class="kw">&lt;ref</span><span class="ot"> follow=</span><span class="st">&quot;name&quot;</span><span class="kw">&gt;&lt;br</span> <span class="kw">/&gt;</span>This paragraph is included into a similar tag and it will be merged into the reference in former paragraph.<span class="kw">&lt;/ref&gt;</span></a>
<a class="sourceLine" id="cb1-7" data-line-number="7"></a>
<a class="sourceLine" id="cb1-8" data-line-number="8">==References== <span class="co">&lt;!--T:89--&gt;</span></a>
<a class="sourceLine" id="cb1-9" data-line-number="9"><span class="kw">&lt;/translate&gt;</span></a>
<a class="sourceLine" id="cb1-10" data-line-number="10"><span class="kw">&lt;references/&gt;</span></a></code></pre></div></td>
<td><p><translate> This is an example of merging multiple texts into the same footnote.[8]</p>
<p><ref follow="main">Simply include the text to be merged into the first <ref name="name"> reference with a tag [9]</translate></p>
<div style="font-weight:bold; font-family:sans-serif;">
<p><translate> References</translate></p>
</div>
<references/></td>
</tr>
</tbody>
</table>

<translate> When using this syntax into Page: namespace, take care that
the ref follow part of the footnote is transcluded too into the same ns0
page, where the first part of footnote is transcluded. Use appropriately
section tags to get this result. To also remind users that reference
names should not solely be numbers, eg. \<ref name="p32"\> and \<ref
follow="p32"\>.

\=== The

<references />

tag ===

The \<tvar|references\>

``` html4strict
<references />
```

\</\> tag inserts the text of all the citations which have defined in
\<tvar|ref\>

``` html4strict
<ref>
```

\</\> tags up to that point in the page. For example, based on the
citations above, there should be reference for the *note*
group.</translate>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p><translate> Wikitext</translate></p></th>
<th><p><translate> Result</translate></p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre class="html4strict"><code>&lt;references group=&quot;note&quot;/&gt;</code></pre></td>
<td><references group="note"/></td>
</tr>
</tbody>
</table>

<translate> In some language editions of Wikipedia, long reference lists
may be placed using the template
`{{`[`Reflist`](w:Template:Reflist "wikilink")`}}`, which incorporates
<code>

<references />

</code>. It provides an optional parameter to display the reference list
in multiple columns. For instance, the English, Hindi and Interlingua
Wikipedias use the css selector `references-small` to make the reference
text smaller than normal text.

If a page includes more than one <code>

<references />

</code> list, each list includes the <ref> tags defined after the
previous references list. If these references lists are produced by
templates, each one lists the ref tags defined before the first
references list, and there is an error message saying that there is a
ref tag but not a references list.

1.  This footnote is used as an example in the "How to use" section.

2.  E. Miller, *The Sun*, (New York: Academic Press, 2005), 23-5.

3.  ''R. Smith, "Size of the Moon", *Scientific American*, 46 (April
    1978): 44-6.

4.
5.  Remember that when you refer to the same footnote multiple times,
    the text from the first reference is used.

6.
7.
8.  Remember that all the texts will be included into the reference
    containing the *name=* attribute.

9.
    This paragraph is included into a similar tag and it will be merged
    into the reference in former paragraph.