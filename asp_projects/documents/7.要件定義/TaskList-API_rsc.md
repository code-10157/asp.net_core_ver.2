
## 現行システム
既存の業務フローとシステムについて調べる

---
### 画面一覧
- <details><summary>ホーム画面</summary>初期画面</details>
- <details>
    <summary>プロジェクト一覧画面</summary>
      案件の管理をする
    <details open>
      <summary>ＣＲＵＤ画面</summary>
        &emsp;&nbsp;新規追加画面&nbsp;<br>
        &emsp;&nbsp;削除画面<br>
        &emsp;&nbsp;編集画面<br>
        &emsp;&nbsp;詳細画面
    </details>
  </details>
- <details>
    <summary>タスク一覧画面</summary>
      案件ごとの作業内容を管理をする
    <details open>
      <summary>ＣＲＵＤ画面</summary>
        &emsp;&nbsp;新規追加画面&nbsp;<br>
        &emsp;&nbsp;削除画面<br>
        &emsp;&nbsp;編集画面<br>
        &emsp;&nbsp;詳細画面
    </details>
  </details>     
- <details><summary>その他画面</summary>未実装。現状は作業の集計機能を有する</details>

---
### 業務フロー


<details>
  <summary>画面遷移図</summary>
  ※ヘッダー表示：Home,Projects,Things

  ``` plantuml
  @startuml
  skinparam backgroundColor white
  skinparam state {
    StartColor skyblue
    EndColor black
  }
  state Home #aliceblue;line:skyblue;line.bold;text:blue
  Home:ホーム画面
  state Projects #aliceblue;line:skyblue;line.bold;text:blue :プロジェクト一覧画面
  state Things #aliceblue;line:skyblue;line.bold;text:blue :タスク一覧画面
  state About #aliceblue;line:skyblue;line.bold;text:blue :その他画面
  About : Home>About

  [*] -[#blue]-> Home
  Home -[#blue]-> Projects
  Home -[#blue]-> Things
  Home -[#blue]-> About

  state CRUD1 #aliceblue;line:skyblue;line.bold;text:blue {
    state Create1 #aliceblue;line:skyblue;line.bold;text:blue :新規追加画面
    state Delete1 #aliceblue;line:skyblue;line.bold;text:blue :削除画面
    state Detail1 #aliceblue;line:skyblue;line.bold;text:blue :詳細画面
    state Edit1 #aliceblue;line:skyblue;line.bold;text:blue :編集画面
  }
  state CRUD2 #aliceblue;line:skyblue;line.bold;text:blue {
    state Create2 #aliceblue;line:skyblue;line.bold;text:blue :新規追加画面
    state Delete2 #aliceblue;line:skyblue;line.bold;text:blue :削除画面
    state Detail2 #aliceblue;line:skyblue;line.bold;text:blue :詳細画面
    state Edit2 #aliceblue;line:skyblue;line.bold;text:blue :編集画面
  }
  Projects -[#blue]-> CRUD1
  Things -[#blue]-> CRUD2

  @enduml
  ```
</details>

<details>
  <summary>ユースケース図</summary>

  ``` plantuml
  @startuml
  left to right direction
  actor "使用者" as ac1

  rectangle TaskList_API {
    ac1 --> (管理する)
    ac1 --> (一覧を表示する)
    ac1 --> (検索する)
    ac1 --> (表示を並べ替える)

    (一覧を表示する) .. (管理する)
    (一覧を表示する) .. (表示を並べ替える)
    (一覧を表示する) .. (検索する)
    (表示を並べ替える) <|-- (昇順で並べ替える)
    (表示を並べ替える) <|-- (降順で並べ替える)
    (管理する) <|-- (プロジェクトを管理する)
    (管理する) <|-- (タスクを管理する)   
    note top of 管理する : CRUD  
  }

  @enduml
  ```
</details>

<details><summary>アクティビティ図</summary>
<details open><summary>- ホーム画面</summary>

  ``` plantuml
  @startuml

  start
  :ホーム画面表示;
  split
     :'Project'押下<
     :プロジェクト一覧画面に遷移;
  end
  split again
     :'Task'押下<
     :タスク一覧画面に遷移;
  end
  split again
     :'Option'押下<
     :その他画面に遷移;
  end

  @enduml
  ```
</details>
<details open><summary>- プロジェクト一覧画面</summary>

  ``` plantuml
  @startuml

  start
  :プロジェクト一覧画面に遷移;
  partition 初期処理 {
      :DB確認;
      if (レコードが存在するか?) then (あり)
      :画面に全件データを表示;
      :ステータス確認;
        if ("完了"?) then (はい)
        :対象行の背景色を灰色にする;
        else (いいえ)
        endif
      else (なし)
      endif
  }
  :プロジェクト一覧画面表示;
  :ボタン選択;
  split
     :'新規追加'押下<
     :Create画面に遷移;
     :画面表示;
     :ボタン選択;
     split
      :'戻る'押下<
     split again
      :'登録'押下<

      if (必須項目記入済み?) then (いいえ)
        :エラーメッセージ表示;
        :Create画面に留まる;
        stop
      endif  
        :DB登録;  
     end split
      :プロジェクト一覧画面に遷移;
  end
  split again
     :'検索'押下<
     if (入力あり?) then (はい)
      :入力ワードで絞り込み;
     else (いいえ)
      :絞り込みなし;
     endif
      :画面にデータを表示;
  end
  split again
     :'絞り込みなし'押下<
     :画面に全件データを表示;
  end
  split again
     :'編集'押下<
     :Edit画面に遷移;
     if (DBに存在するデータを選択?) then (いいえ)
     :エラー画面表示;
     stop
     endif
     :画面表示;
     split
      :'戻る'押下<
     split again
      :'保存'押下<
      :画面に入力された内容でDBを更新;
     end split
      :プロジェクト一覧画面に遷移;
  end
  split again
     :'詳細'押下<
     :Detail画面に遷移;
     if (DBに存在するデータを選択?) then (いいえ)
      :エラー画面表示;
     stop
     endif
       :画面表示;
       :選択したデータの内容を表示;
       :データのステータス確認;
     if (完了?) then (はい)
      :プロジェクトに属するタスク一覧DBの
      データを全件表示;
     endif
     split
      :'編集へ'押下<
      :編集画面に遷移;
     split again
      :'一覧へ'押下<
      :プロジェクト一覧画面に遷移;
     end split

  end
  split again
     :'削除'押下<
     :Delete画面に遷移;
     if (DBに存在するデータを選択?) then (いいえ)
     :エラー画面表示;
     stop
     endif
     :画面表示;
     split
      :'削除'押下<
      :選択しているレコードをDBから削除;
     split again
      :'戻る'押下<
     end split
      :プロジェクト一覧画面に遷移;
  end

  @enduml
  ```
</details>
<details open><summary>- タスク一覧画面</summary>

  ``` plantuml
  @startuml

  start
  :タスク一覧画面に遷移;
  partition 初期処理 {
      :DB確認;
      if (レコードが存在するか?) then (あり)
      :画面に全件データを表示;
      :ステータス確認;
        if ("完了"?) then (はい)
        :対象行の背景色を灰色にする;
        else (いいえ)
        endif
      else (なし)
      endif
  }
  :タスク一覧画面表示;
  :ボタン選択;
  split
     :'新規追加'押下<
     :Create画面に遷移;
     :画面表示;
     :ボタン選択;
     split
      :'戻る'押下<
     split again
      :'登録'押下<

      if (必須項目記入済み?) then (いいえ)
        :エラーメッセージ表示;
        :Create画面に留まる;
        stop
      endif  
        :DB登録;  
     end split
      :プロジェクト一覧画面に遷移;
  end
  split again
     :'検索'押下<
     if (入力あり?) then (はい)
      :入力ワードで絞り込み;
     else (いいえ)
      :絞り込みなし;
     endif
      :画面にデータを表示;
  end
  split again
     :'絞り込みなし'押下<
     :画面に全件データを表示;
  end
  split again
     :'編集'押下<
     :Edit画面に遷移;
     if (DBに存在するデータを選択?) then (いいえ)
     :エラー画面表示;
     stop
     endif
     :画面表示;
     split
      :'戻る'押下<
     split again
      :'保存'押下<
      :画面に入力された内容でDBを更新;
     end split
      :プロジェクト一覧画面に遷移;
  end
  split again
     :'詳細'押下<
     :Detail画面に遷移;
     if (DBに存在するデータを選択?) then (いいえ)
      :エラー画面表示;
     stop
     endif
       :画面表示;
       :選択したデータの内容を表示;
     split
      :'編集へ'押下<
      :編集画面に遷移;
     split again
      :'一覧へ'押下<
      :プロジェクト一覧画面に遷移;
     end split

  end
  split again
     :'削除'押下<
     :Delete画面に遷移;
     if (DBに存在するデータを選択?) then (いいえ)
     :エラー画面表示;
     stop
     endif
     :画面表示;
     split
      :'削除'押下<
      :選択しているレコードをDBから削除;
     split again
      :'戻る'押下<
     end split
      :プロジェクト一覧画面に遷移;
  end

  @enduml
  ```
</details>
<details open><summary>- その他画面</summary>

  ``` plantuml
  @startuml

  start
  :その他画面遷移;
  :DB:projectsを確認;
  if (データあり?) then (いいえ)
    :その他画面表示;
    stop
  endif  
  :項目名:開始日でデータをグループ化;
  :開始日と件数を画面に表示;
  :その他画面表示;
  end

  @enduml
  ```
</details>

</details>

---
### データ構造

- テーブル
  - Project：プロジェクトデータを管理する
  - Thing：タスクデータを管理する
<br>

- データ
  - Project
    |項目名|画面表示名|データ型|桁数|null|PrimaryKey|備考|
    |:--:|:--:|:--:|:--:|:--:|:--:|:--:|
    |ID|-|int|-|NOTNULL|○|画面表示されない|
    |Category|分類|int|-|NOTNULL||enum|
    |ProjectName|プロジェクト名|nvarchar|50|NOTNULL|||
    |StartDate|開始日|datetime2|7|NOTNULL|||
    |CompletionDate|完了日|datetime2|7|NOTNULL|||
    |Status|状態|int|-|-|||
    |Priority|優先度|int|-|NOTNULL||enum|
    |Comment|備考|nvarchar|MAX|-|-||

  - Thing
    |項目名|画面表示名|データ型|桁数|null|PrimaryKey|備考|
    |:--:|:--:|:--:|:--:|:--:|:--:|:--:|
    |ID|-|int|-|NOTNULL|○|画面表示されない|
    |ProjectID|プロジェクト名|int|-|NOTNULL|-|外部キー:Project.ID|
    |Process|工程|nvarchar|MAX|NOTNULL|-||
    |TaskName|作業名|nvarchar|MAX|NOTNULL|-||
    |Start|着手日|datetime2|7|NOTNULL|-||
    |End|完了日|datetime2|7|NOTNULL|-||
    |Status|状態|int|-|-|-|enum|
    |Progress|進捗|int|MAX|NOTNULL|-||
    |Memo|メモ|nvarchar|MAX|NOTNULL|-||


---
### 現行システムの課題

- ホーム画面
  - 機能が少ない
<br>

- プロジェクト一覧画面
  - 検索機能改善
    - 「絞り込みなし」の表示名が実際の動きとは少し異なる
    - キーワード入力なしで検索を押下すると全レコードが表示される
  - ソート機能改善
    - 初期状態では並べ替えされていないため期限や優先度などが一目でわからない
  - 備考欄は初期画面の見やすさを考慮し詳細画面で表示させるべき。長い文章が登録されていた場合、一覧画面のレイアウトが崩れる。
<br>

- タスク一覧画面
  - 検索機能改善
    - 「絞り込みなし」の表示名が実際の動きとは少し異なる
    - キーワード入力なしで検索を押下すると全レコードが表示される
  - ソート機能改善
    - 初期状態では並べ替えされていないため期限や優先度などが一目でわからない
    - 工程を自由に記載できるのは便利だが、ソートは上手く機能しない
  - メモは初期画面の見やすさを考慮し詳細画面で表示させるべき。長い文章が登録されていた場合、一覧画面のレイアウトが崩れる。
<br>

- その他画面
  - 用途が定まっていない
  - 集計機能が中途半端
  - 画面名の意味がわかりづらい
<br>

- 現状は使用者一人の情報しか管理できない
- セキュリティ面の考慮が一切ない

---
### 改善案

- ホーム画面<br>
→カレンダーやガントチャート、当日期限のタスクなどを表示させるようにする
<br>

- プロジェクト一覧画面<br>
→「絞り込みなし」の表示を「_全データ表示_」に変更する<br>
→キーワード入力なしで検索を押下した際の挙動は現状のままで問題ない<br>
→画面開設の初期処理で「_分類_」「_期限_」でデータを並べ替える機能を追加する<br>
→「_備考_」を一覧画面の表示から外し、_詳細画面で表示させる_
<br>

- タスク一覧画面<br>
→「絞り込みなし」の表示を「_全データ表示_」に変更する<br>
→キーワード入力なしで検索を押下した際の挙動は現状のままで問題ない<br>
→画面開設の初期処理で「_分類_」「_工程_」「_期限_」でデータを並べ替える機能を追加する<br>
→「_工程_」を自由記入から*enum値から選択する方式*に変更する<br>
→「_メモ_」を一覧画面の表示から外し、*詳細画面で表示させる*
<br>

- その他画面<br>
→今後検討していく
<br>

- ユーザを管理するテーブルを新規で追加し、所属チームの各メンバーの作業状況を確認可能とする<br>
- ユーザ区分を「管理者」と「一般」に分け、「管理者」にユーザ管理の権限を付与する<br>
- ログイン機能を追加<br>
- アラーム機能を追加し、作業状況に問題がある場合は「管理者」に通知を送れるようにする
