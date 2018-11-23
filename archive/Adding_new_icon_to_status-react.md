tags: svg icons status-react

## How to add new svg icon and use it in status-react project

First of all, you need to get svg file for icon

#### Select necessary icon in Zeplin

and click save icon

![zeplin-svg.png](zeplin-svg.png "zeplin-svg.png")

#### Copy file into resources folder

<https://github.com/status-im/status-react/tree/develop/resources/icons>

#### Make sure svg file has proper structure

for example, the first tag should be **svg**, there should be no extra
tags, supported tags : **svg, g, rect, path, use-def, defs**

For example, this
file

<?xml version="1.0" encoding="UTF-8"?>

<svg width="693px" height="199px" viewBox="0 0 693 199" version="1.1" ns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">

<title>

Combined
Shape@2x

</title>

`    `<desc>`Created with Sketch.`</desc>
`    `<defs></defs>
`    `<g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
`        `<g id="Artboard" transform="translate(-445.000000, -575.000000)" fill-rule="nonzero" fill="#4360DF">
`            `<path d="M678.460656,697.906856 L703.244656,697.906856 C703.244656" id="Combined-Shape"></path>
`        `</g>
`    `</g>

</svg>

should be modified
to

<svg >

`            `<path fill="#4360DF" d="M678.460656,697.906856 L703.244656,697.906856 C703.244656"/>

</svg>

make sure there is **fill** property exists. Also, there are some
'''limitations '''

Pattern element. Mask element. Marker element. '''Transform prop are not
supported
'''.

#### Add icon into icons map

<https://github.com/status-im/status-react/blob/develop/src/status_im/components/icons/vector_icons.cljs#L26>

`(def icons {:icons/icon-name (slurp-svg
"./resources/icons/icon-name.svg")`

#### Use icon in the code

<code> \[status-im.components.icons.vector-icons :as vector-icons\]

\[vector-icons/icon :icons/icon-name {:color :red}\]\]

</code>

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")