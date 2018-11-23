本文档列出了开发团队使用的一系列约定，请参阅[此指南](https://github.com/bbatsov/clojure-style-guide)查找此处未涉及的任何内容。

## 变量命名

### 使用'

避免在变量名称中使用附加的`'`(prime)，因为这很难察觉，特别是在视觉受损的情况下。

``` clojure

  (defn do-stuff [thing]
    (let [thing' (do-some-other-stuff thing)]))
```

首选线程，有意义的名称或者不可能有更多可读的变量名称。

## 汇入的名称空间

### 使用 `:as` 和`:refer`

避免使用 `:refer`，而应该使用 `:as` .

作为这个规则的一个例外，我们通常为test.cljs使用 :refer 导入macros.

### 命名空间别名

如果不明确，请使用点分隔的名称，例如：`status.ui.utils.clocks :as utils.clocks`而不是`：as
clocks`或`：as utils-clocks`

## 注释

在评论的代码上方添加注释，而不是在右侧。

``` clojure

; good
(do-stuff)

(do-stuff) ; bad
```

## 缩进

### 地图

缩进地图，以便所有值落在同一列上：

``` clojure

{:very-long-key "a"
 :short-key     "b"}
```

## 重新框架

### 优先考虑功能组合，而不是多次调度

如果可能，最好使用函数组合而不是调度。
您可以利用`merge-fx`宏，该宏假定函数将`cofx`作为最后一个位置参数，并返回`fxs`（或`nil`）将被合併回去。

实际上，函数/处理程序应该如下所示：

``` clojure

  (defn do-stuff [cofx]
    (merge-fx cofx
      db-fn
      some-other-fx-fn))
```

相对的：

``` clojure

(defn do-stuff [fx]
    (-> fx
      (update  :db db-fn)
      (merge {:dispatch [:some-other-fx]})
```

因此在编写新功能时：

1.  让他们返回一个`fxs`的地图。
2.  有`cofx`作为最后一个参数，以便其他人可以通过`merge-fx`使用它们。
3.  操作`cofx`而不是`db`，以便可以对新函数进行线程化。

你可以阅读[这个](https://github.com/status-im/ideas/issues/31)。

### 使用有意义的事件和订阅名称。

尽可能避免使用 `[:get :a]` 或`[:set :a]`并且倾向于使用传达您意图的名称。

例如： `[:set :tab-visible true]`

可以是:

`[:show-tab]`

和

`[:get :tab-visible]`

可以是:

`[:tab-visible?]`

后备时间:

`[:set-tab-visible]` `[:get-tab-visible]`

如果你想不出更好的东西。

### 异步流

优先使用`fx`并调度异步控制流，而不是使用像`core.async`这样的库。

对于一些灵感：

  - <https://github.com/Day8/re-frame-http-fx>
  - <https://github.com/Day8/re-frame-async-flow-fx>

[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")
[Category:Style guide](Category:Style_guide "wikilink")