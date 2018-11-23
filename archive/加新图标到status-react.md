# 如何添加新的svg图标并在status-react project中使用它

首先，你需要获得svg文件的图标

## 在Zeplin中选择必要的图标

和点击保存svg图标

![zeplin-svg.png](zeplin-svg.png "zeplin-svg.png")

## 将文件複製到资源文件夹

<https://github.com/status-im/status-react/tree/develop/resources/icons>

## 确保svg文件具有正确的结构

例如，第一个标记应该是**svg**，应该没有额外的标记，支持的标记：**svg，g，rect，path，use-def，defs**

例如，这个文件

```
 <?xml version="1.0" encoding="UTF-8"?>
 <svg width="693px" height="199px" viewBox="0 0 693 199" version="1.1" ns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
     <!-- Generator: Sketch 46.2 (44496) - http://www.bohemiancoding.com/sketch -->
     <title>Combined Shape@2x</title>
     <desc>Created with Sketch.</desc>
     <defs></defs>
     <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
         <g id="Artboard" transform="translate(-445.000000, -575.000000)" fill-rule="nonzero" fill="#4360DF">
             <path d="M678.460656,697.906856 L703.244656,697.906856 C703.244656" id="Combined-Shape"></path>
         </g>
     </g>
 </svg>
```

should be modified to

```
 <svg >
             <path fill="#4360DF" d="M678.460656,697.906856 L703.244656,697.906856 C703.244656"/>
 </svg>
```

确保存在**fill**属性。此外，还有一些**局限**

Pattern element. Mask element. Marker element. '''Transform prop are not
supported
'''.

## 加图标到图标地图中

<https://github.com/status-im/status-react/blob/develop/src/status_im/components/icons/vector_icons.cljs#L26>

`(def icons {:icons/icon-name (slurp-svg
"./resources/icons/icon-name.svg")`

## 在代码中使用图标

<code> \[status-im.components.icons.vector-icons :as vector-icons\]

\[vector-icons/icon :icons/icon-name {:color :red}\]\]

</code> [category:svg](category:svg "wikilink")
[category:icons](category:icons "wikilink")
[category:status-react](category:status-react "wikilink")

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")