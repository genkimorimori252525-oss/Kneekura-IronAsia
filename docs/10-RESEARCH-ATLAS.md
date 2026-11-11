# MOD・技術調査アトラス (2026-11-11)
外部作品のソース/構造/既存問題から**発想・設計パターン・失敗例**を学ぶ場所。コピーや導入を前提としない。README/説明文からの確認と、ソース読解・実ゲーム検証は別段階。

## 調査ラベル
- `SOURCE-CLAIM`: 作者/公式の機能説明を確認（動作実証ではない）
- `CODE-REVIEW-TODO`: 対象ファイル・差分と実装機構の読解は未実施
- `EXPERIMENT-TODO`: 実機性能・互換・成功率は未測定
- `IDEA`: IronAsia用の仮説/提案。既存MODが実装済みだとは主張しない

| 参考先 | 確認したこと / 状態 | IronAsiaの採取対象 | 避けたい反面教師 |
|---|---|---|---|
| [Millennium Dawn](https://github.com/MillenniumDawn/Millennium-Dawn) | SOURCE-CLAIM: 経済、影響力、国際組織、現代技術 | 国際関係モデル、外交と経済の接続 | マイクロ管理や重さを無検証で継承しない |
| [MD International Systems](https://github.com/MillenniumDawn/Millennium-Dawn/blob/main/docs/src/content/tutorials/international-systems.md) | SOURCE-CLAIM: 国連、NATO、アフリカ連合、制裁など | 国際機関の加盟・投票・執行の分離 | 1936年に国連をそのまま置かない |
| [Cold War: Iron Curtain](https://github.com/Cold-War-Iron-Curtain-Dev-Team/ColdWarIronCurtain) | SOURCE-CLAIM: 国連、核緊張、経済、長期時間軸、細密地図 | 戦後国際秩序/ゲーム期間延長 | 長大技術ツリーの操作量と性能 |
| [The Road to 56](https://steamcommunity.com/sharedfiles/filedetails/?id=820260968) | SOURCE-CLAIM: 長い技術ツリー、架空歴史分岐、小国NF | リプレイ性と小国向けの厚み | 分岐を増やすだけで選択肢を水増ししない |
| [The New Order](https://steamcommunity.com/workshop/filedetails/?id=2438003901) | SOURCE-CLAIM: 内政・政治経済GUIと大量イベント | 物語型外交と制度変化の表現 | 情報過多・恒常的なクリック労働 |
| [Historical States](https://steamcommunity.com/sharedfiles/filedetails/?id=3129639972) | SOURCE-CLAIM: 大陸全域、特に東アジアの州/プロヴィンス改修 | 国境の分解能と歴史再現 | 全地球一律の高密度化 |
| [HOI4 Map Editor](https://github.com/AdriianCOE/HOI4-Map-Editor) | SOURCE-CLAIM: プロヴィンス・州の編集、検査、バックアップ | 地図制作・参照整合性の検証 | マップを無検証で一括更新しない |
| [Tech Baseline Mechanics](https://github.com/henri14871/hoi4-tech-baseline-mechanics) | SOURCE-CLAIM: 技術底上げ、追いつき・研究枠外の進展、負荷分散 | 小国の研究格差、分散更新、ゲームルール | 無料技術付与による研究戦略の消滅 |
| [AI modding reference](https://github.com/thelight0211/hoi4-wiki/blob/main/AI%20modding%20-%20Hearts%20of%20Iron%204%20Wiki.md) | リファレンス: 国家AIの研究重み・外交・前線重み | 設定できるAIの境界を特定 | 人間側の将軍に独立AIを作れると誤認しない |
| [Map modding reference](https://github.com/thelight0211/hoi4-wiki/blob/main/Map%20modding%20-%20Hearts%20of%20Iron%204%20Wiki.md) | リファレンス: provinces.bmp、definition.csv、state/supply接続 | province ID管理、検証、地図コスト計測 | 色/ID/路線の参照破壊 |
| [CWTools](https://github.com/cwtools/cwtools-vscode) / [HOI4 config](https://github.com/cwtools/cwtools-hoi4-config) | SOURCE-CLAIM: 構文・参照・スコープ・ローカライズ検査 | 最小限の静的チェック | 静的検査だけでゲーム動作を認定しない |
| [Agentic HOI4 Modding](https://github.com/klimPaskov/Agentic-HOI4-Modding) / [Agent Tools](https://github.com/klimPaskov/hoi4-agent-tools) | SOURCE-CLAIM: AI作業手順、MCP、ツリー/イベント/地図の検査とプレビュー | 必要時のみスキルとツール採用 | 使わないMCP常駐・すべての道具を毎回使用 |
| [HOI4 Live Tools](https://github.com/klimPaskov/HOI4-Live-Tools) | SOURCE-CLAIM: Windows版自動起動・イベント・ログ等、完全自動キャンペーン未認定 | 段階的な実ゲーム再現検証 | READMEの機能一覧を実証済み扱い |
| [Extended Timeline](https://github.com/ExtendedTimeline/ExtendedTimeline) | SOURCE-CLAIM: 大戦終結状況で戦後シナリオを分岐 | 1945年以降への動的移行 | 史実一択・固定の終戦条件 |

## 追加調査テンプレート
- Source URL / 固定コミット・版 / 閲覧日 / ライセンス
- 観察事実 (FACT) / 作者の説明 (CLAIM) / 推論 (INFERENCE) / 提案 (IDEA)
- 採用する問題解決パターン（具体的ファイル・仕組み・エンジンAPI）
- 非採用・重複理由
- 性能費用、セーブ影響、DLC/版、国別偏り、UX複雑性
- 最小再現実験、成功/失敗ログ、撤回条件、メンテナンス担当

## ソースの取り扱い
- 第三者のMOD全体を「取り込む」と決めない。参照/研究とソース・アセット複製は別。
- ソース/画像/翻訳/3Dアセットには著作権とライセンスがある。個別の許諾・条件・帰属を確認してから採用。
- 他MODとの同居を目指す場合も、地図/国家定義/技術ツリーは衝突しやすい。まず独立動作を確立。
