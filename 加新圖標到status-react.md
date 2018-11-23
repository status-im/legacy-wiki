# 如何添加新的svg圖標並在status-react project中使用它

首先，你需要獲得svg文件的圖標

## 在Zeplin中選擇必要的圖標

和點擊保存svg圖標

![zeplin-svg.png](zeplin-svg.png "zeplin-svg.png")

## 將文件複製到資源文件夾

<https://github.com/status-im/status-react/tree/develop/resources/icons>

## 確保svg文件具有正確的結構

例如，第一個標記應該是**svg**，應該沒有額外的標記，支持的標記：**svg，g，rect，path，use-def，defs**

例如，這個文件

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

確保存在**fill**屬性。此外，還有一些**局限**

Pattern element. Mask element. Marker element. '''Transform prop are not
supported
'''.

## 加圖標到圖標地圖中

<https://github.com/status-im/status-react/blob/develop/src/status_im/components/icons/vector_icons.cljs#L26>

`(def icons {:icons/icon-name (slurp-svg
"./resources/icons/icon-name.svg")`

## 在代碼中使用圖標

<code> \[status-im.components.icons.vector-icons :as vector-icons\]

\[vector-icons/icon :icons/icon-name {:color :red}\]\]

</code> [category:svg](category:svg "wikilink")
[category:icons](category:icons "wikilink")
[category:status-react](category:status-react "wikilink")

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")