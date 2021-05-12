## 要件定義書
各要件についてまとめる

---

### バージョン

- ver.1.0：20yy/mm/dd 新規作成

---

### １．概要

#### システム構成図

--

#### 当プロジェクトの関係者

- 〇〇：～担当
- ✖✖：～担当
- △△：～担当

#### 顧客の要求

- [ ] 進捗管理を単純かつ容易にする
- [ ] 進捗管理に費やす時間の削減（作業効率化）
- [ ] 各チーム／各要員が抱える作業の可視化
- [ ] 各人の作業状況が一目で確認できるようにする
- [x] 対応漏れの発見

#### 背景

現在はチームメンバが各自で進捗の管理をしている。しかし管理方法に決まりがないためにチーム全体の作業状況を確認するとなると時間がかかる。
進捗確認の時間を短縮し業務に使える時間を増加させたいと考えている。その手段として新たに進捗管理システムを構築し、作業状況の記録及び確認を効率化させることを目指す。

---
### ２．業務要件

#### 業務フロー

水色：実装済み
橙色：未実装

![簡易業務フロー](https://github.com/code-10157/asp.net_core_ver.2/blob/main/asp_projects/documents/%E8%B3%87%E6%96%99/%E6%A5%AD%E5%8B%99%E3%83%95%E3%83%AD%E3%83%BC.png)


#### 規模
１名～数十名で使用する程度の規模


#### 時期・時間
開発期間は１年（１２ヵ月）とする
段階を分けて開発を行う
１日２時間×３６５日
→総時間：８３０時間

- 第１段階（期間：７ヵ月）
  - 要件定義：2ヵ月
  - 基本設計：2ヵ月
  - プログラミング：2ヵ月
  - テスト：1ヵ月

<br>

- 第２段階（期間：５ヵ月）
  - 要件定義：1ヵ月
  - 基本設計：1ヵ月
  - プログラミング：2ヵ月
  - テスト：1ヵ月

<details>
  <summary>ガントチャート</summary>

  __-第１段階-__ <br>
  要件定義

  ``` plantuml
  @startgantt
  -- 第１段階 --
  [要件定義] lasts 60 days
  -- タスク --
  [資料確認] lasts 5 days
  then [現行システムの調査] lasts 10 days
  then [対応方針の検討] lasts 15 days
  then [仕様書作成] lasts 15 days
  then [レビュ―] lasts 10 days
  then [予備] lasts 5 days
  @endgantt
  ```

  基本設計

  ``` plantuml
  @startgantt
  -- 第１段階 --
  [基本設計] lasts 60 days
  -- タスク --
  [設計書作成] lasts 30 days
  then [試験書作成] lasts 15 days
  then [レビュ―] lasts 15 days
  @endgantt
  ```

  プログラミング

  ``` plantuml
  @startgantt
  -- 第１段階 --
  [プログラミング] lasts 60 days
  -- タスク --
  [新規機能の実装１] lasts 15 days
  then [新規機能の実装２] lasts 15 days
  then [新規機能の実装３] lasts 15 days
  then [レビュ―] lasts 15 days
  @endgantt
  ```

  テスト

  ``` plantuml
  @startgantt
  -- 第１段階 --
  [テスト] lasts 30 days
  -- タスク --
  [試験実施] lasts 30 days
  [バグの修正] lasts 30 days
  @endgantt
  ```

  __-第２段階-__ <br>
  要件定義
  ``` plantuml
  @startgantt
  -- 第１段階 --
  -- 第２段階 --
  [要件定義] lasts 30 days
  -- タスク --
  [対応方針の検討] lasts 12 days
  then [仕様書の修正] lasts 12 days
  then [レビュ―] lasts 6 days
  @endgantt
  ```

  基本設計
  ``` plantuml
  @startgantt
  -- 第１段階 --
  -- 第２段階 --
  [基本設計] lasts 30 days
  -- タスク --
  [設計書作成] lasts 20 days
  then [試験書作成] lasts 5 days
  then [レビュ―] lasts 5 days
  @endgantt
  ```

  プログラミング
  ``` plantuml
  @startgantt
  -- 第１段階 --
  -- 第２段階 --
  [プログラミング] lasts 60 days
  -- タスク --
  [新規機能の実装１] lasts 15 days
  then [新規機能の実装２] lasts 15 days
  then [新規機能の実装３] lasts 15 days
  then [レビュ―] lasts 15 days
  @endgantt
  ```

  テスト
  ``` plantuml
  @startgantt
  -- 第１段階 --
  -- 第２段階 --
  [テスト] lasts 25 days
  -- タスク --
  [試験実施] lasts 25 days
  [バグの修正] lasts 25 days
  @endgantt
  ```

  __-リリース-__
  ``` plantuml
  @startgantt
  -- 第１段階 --
  -- 第２段階 --
  -- リリース --
  [リリース] lasts 5 days
  @endgantt
  ```

</details>
