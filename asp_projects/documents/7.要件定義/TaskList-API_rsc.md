
## 現行システム
既存の業務フローとシステムについて調べる

---
### 画面一覧
- <details><summary>ホーム画面</summary>初期画面</details>
- <details><summary>プロジェクト一覧画面</summary>案件の管理をする</details>

  - 新規追加画面
  - 削除画面
  - 編集画面
  - 詳細画面
- <details><summary>タスク一覧画面</summary>案件ごとの作業内容を管理をする</details>     

  - 新規追加画面
  - 削除画面
  - 編集画面
  - 詳細画面
- <details><summary>その他画面</summary>未実装。現状は作業の集計機能を有する</details>

---
### 業務フロー
<details>
  <summary>ユースケース図</summary>

  ``` plantuml
  @startuml
  left to right direction
  actor "使用者" as ac1

  rectangle TaskList_API {
    ac1 --> (登録する)
    ac1 --> (削除する)
    ac1 --> (一覧を表示する)
    ac1 --> (検索する)
    ac1 --> (編集する)
    ac1 --> (表示を並べ替える)

    (一覧を表示する) .. (登録する)
    (一覧を表示する) .. (削除する)
    (一覧を表示する) .. (表示を並べ替える)
    (一覧を表示する) .. (検索する)
    (一覧を表示する) .. (編集する)
    (表示を並べ替える) <|-- (昇順で並べ替える)
    (表示を並べ替える) <|-- (降順で並べ替える)
  }
  @enduml
  ```
</details>

<details><summary>アクテビティ図</summary>

``` plantuml
@startuml

start
:ClickServlet.handleRequest();
:new page;
if (Page.onSecurityCheck) then (true)
  :Page.onInit();
  if (isForward?) then (no)
    :Process controls;
    if (continue processing?) then (no)
      stop
    endif

    if (isPost?) then (yes)
      :Page.onPost();
    else (no)
      :Page.onGet();
    endif
    :Page.onRender();
  endif
else (false)
endif

if (do redirect?) then (yes)
  :redirect process;
else
  if (do forward?) then (yes)
    :Forward request;
  else (no)
    :Render page template;
  endif
endif

stop

@enduml
```
</details>
