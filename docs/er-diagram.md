以下にため池GO! における、データベースのER図を示す。
なお、画像は[Mermaid Live Editor](https://mermaid.live/edit)にて出力した。

```mermaid
---
Title: "ため池GO! のデータベース設計"
---
erDiagram
    Users ||--o{ GetStamps : contains
    GetStamps ||--o{ Ponds : places
    Users ||--o{ GetAnimals : contains
    GetAnimals ||--o{ Animals : species

    Users {
        string(6) uid PK "ユーザID"
        int myScore "スタンプで取得したポイント"
    }

    GetStamps {
        string(6) index PK "スタンプを特定する"
        %% 注意点として、Post処理にて同一データをN回保存しても、重複に気づかないリスクがある
        string(6) uid FK "ユーザID"
        int pondKey FK "スタンプを取得した、ため池を判別するため"
        Date getStampAt "スタンプを取得したした日時"
    }

    %% ⚠️Ponds Classはデータベースに保存しない
    %% あくまでコードロジックに反映させるためのモデル
    Ponds {
        int pondKey PK "ため池を識別するためのキー"
        string name "ため池の名前が重複する場合がある(e.g. 皿池)"
        float latitude PK "緯度を表す"
        float longitude PK "経度を表す"
        string url "ため池ミュージアムのため池紹介ページリンク"
        string imagePath "画像を設置しているパス"
    }

    GetAnimals {
        int index PK "取得した動物の記録"
        string(6) uid FK "取得したユーザの識別"
        int animalKey FK "動物を識別するためのID"
        Date getAnimalAt "動物をガチャから取得した日時"
    }

    %% Animals Classも同様にデータベースへ保存しない
    Animals {
        int animalKey PK "動物を識別するためのID"
        string name PK "動物の名前"
        string spiecies "分類（虫、魚、鳥など）"
        string livingArea "生息地"
        text description "動物の説明"
        string imagePath "動物画像のパス"
    }
```
