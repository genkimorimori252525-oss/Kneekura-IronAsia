# データ契約・セーブ・歴史履歴（提案）
2026-11-11 / 設計案。例示JSONはHOI4ランタイムが直接読書きできる保証のある形式ではない。

## データを3種類に分ける
1. **Authoring Data**: 開発用のJSON/CSV/史料等。検証済みの変換器がPDXスクリプトやlocalisationへ変換する。設計図でありセーブデータではない。
2. **Live Save State**: HOI4エンジンが持つゲーム状態 + MOD内でサポートされる変数・フラグ等。保存と復元は実機テストが必要。ゲーム側の標準形式と寿命に従う。
3. **External History Report**: セーブなどから外部の検証済みツールで生成するJSON/年表。ランタイムMODが外部ファイルを自由に書き出す前提にしない。

## ゲーム内履歴のデータモデル（概念）
重要な歴史の要素: who, affected, when, cause, previous state, choice, outcome, source.
- 事件: 国境要求・拒否・譲歩・外交関係の転機・同盟・制裁・仲裁・講和・独立・政権移行などの**重要な変化**。
- 危機ID、参加国、地域・州、開始日、段階、最終解決、解決理由を残す。
- すべての国家ペア/全日次値を保存しない。進行中の危機のみ詳細、完了済みは要約/主要転機の最小記録に縮約。
- セーブの際には関係状態・進行中案件・技術の自動方針・コントロール権限の復元が優先。
- 全履歴の永続性を実装できない場合、その制約をUIに示し「ゲーム内重要履歴」と「外部年表」を区別。

## 開発・外部エクスポータ用の概念JSON例
```json
{
  "schema": "ironasia.history.v0-proposal",
  "gameVersion": "UNVERIFIED",
  "modRevision": "UNVERIFIED",
  "observedAt": "in-game-date",
  "records": [
    {
      "id": "territory-case-example",
      "kind": "territorial_dispute",
      "parties": ["AAA", "BBB"],
      "stateRefs": ["STATE_ID_TO_VERIFY"],
      "status": "mediated",
      "causes": ["claim", "international_mediation"],
      "outcome": "negotiation",
      "evidence": "source_or_game_event_id",
      "certainty": "example_only"
    }
  ]
}
```
ここに示すタグ/IDは完全にダミーで、ゲームに投入する実データではない。実際の事件と参照を検証したうえで変換する。

## 保存可能性の実験
1. 新規ゲーム開始→領土係争を発生させる→auto/manualの設定を変更。
2. セーブ→ゲーム終了→ロード→危機の状態、国家間関係、方針設定を検証。
3. 100件・1000件相当の重要イベントでロード時間とセーブ容量を測定。
4. 外部JSONエクスポータが必要なら別サブプロジェクトを設計し、ユーザーの任意利用とする。
5. 保存・復元不可能な部分を発見したら機能の受入を止めて別方式を検討。

## スキーマ設計原則
- 安定したID、世界時刻と記録時刻の分離、明示的なschemaVersion、履歴の証拠・由来。
- 史実データ（出典/確度）とゲーム内の架空歴史（実行ログ）を混同しない。
- 将来の地図変更でprovince/state IDは変わり得る。地理IDのマイグレーションが必要。
- 国際関係の点数を保存する場合も、その点数を構成する主要理由を復元可能にする。
- 外部ツールを使わないユーザーにもゲーム内で重要な経緯が理解できる。
