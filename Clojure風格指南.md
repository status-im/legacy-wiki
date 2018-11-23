本文檔列出了開發團隊使用的一系列約定，請參閱[此指南](https://github.com/bbatsov/clojure-style-guide)查找此處未涉及的任何內容。

## 變量命名

### 使用'

避免在變量名稱中使用附加的`'`(prime)，因為這很難察覺，特別是在視覺受損的情況下。

``` clojure

  (defn do-stuff [thing]
    (let [thing' (do-some-other-stuff thing)]))
```

首選線程，有意義的名稱或者不可能有更多可讀的變量名稱。

## 匯入的名稱空間

### 使用 `:as` 和`:refer`

避免使用 `:refer`，而應該使用 `:as` .

作為這個規則的一個例外，我們通常為test.cljs使用 :refer 導入macros.

### 命名空間別名

如果不明確，請使用點分隔的名稱，例如：`status.ui.utils.clocks :as utils.clocks`而不是`：as
clocks`或`：as utils-clocks`

## 註釋

在評論的代碼上方添加註釋，而不是在右側。

``` clojure

; good
(do-stuff)

(do-stuff) ; bad
```

## 縮進

### 地圖

縮進地圖，以便所有值落在同一列上：

``` clojure

{:very-long-key "a"
 :short-key     "b"}
```

## 重新框架

### 優先考慮功能組合，而不是多次調度

如果可能，最好使用函數組合而不是調度。
您可以利用`merge-fx`宏，該宏假定函數將`cofx`作為最後一個位置參數，並返回`fxs`（或`nil`）將被合併回去。

實際上，函數/處理程序應該如下所示：

``` clojure

  (defn do-stuff [cofx]
    (merge-fx cofx
      db-fn
      some-other-fx-fn))
```

相對的：

``` clojure

(defn do-stuff [fx]
    (-> fx
      (update  :db db-fn)
      (merge {:dispatch [:some-other-fx]})
```

因此在編寫新功能時：

1.  讓他們返回一個`fxs`的地圖。
2.  有`cofx`作為最後一個參數，以便其他人可以通過`merge-fx`使用它們。
3.  操作`cofx`而不是`db`，以便可以對新函數進行線程化。

你可以閱讀[這個](https://github.com/status-im/ideas/issues/31)。

### 使用有意義的事件和訂閱名稱。

盡可能避免使用 `[:get :a]` 或`[:set :a]`並且傾向於使用傳達您意圖的名稱。

例如： `[:set :tab-visible true]`

可以是:

`[:show-tab]`

和

`[:get :tab-visible]`

可以是:

`[:tab-visible?]`

後備時間:

`[:set-tab-visible]` `[:get-tab-visible]`

如果你想不出更好的東西。

### 異步流

優先使用`fx`並調度異步控制流，而不是使用像`core.async`這樣的庫。

對於一些靈感：

  - <https://github.com/Day8/re-frame-http-fx>
  - <https://github.com/Day8/re-frame-async-flow-fx>

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")
[Category:Style guide](Category:Style_guide "wikilink")