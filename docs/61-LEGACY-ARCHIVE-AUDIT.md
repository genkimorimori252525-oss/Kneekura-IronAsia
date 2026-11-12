# KneekuraEngine 旧原本の静的監査（2026-11-12）

> この資料は所有者提供 `KneekuraEngine.zip` からの**静的読解**。HOI4上で起動・検証した記録ではない。旧実装を新リポへ同梱していない。

## 原本の同定と完全性
- 提供ファイル: `KneekuraEngine.zip` （チャット添付）。
- SHA-256: `de5eaaad7bf320a4f845e482ddeb2c42dd87be9d7757fd9f672de66c1895fbd0`
- 圧縮ファイル 351 KiB相当（実測 351K 表示）、展開後総サイズ 1,136,014 bytes、124ファイル（ディレクトリ除外）。
- 内訳: 71 × `.txt`（ゲーム定義）、32 × `.yml`（日本語/英語表示）、11 × `.py`（ローカライズ生成ツール）、ほか `descriptor.mod`、Lua、GUI、画像、IDE設定など。
- `descriptor.mod`: `name = "KneekuraEngine"`、`supported_version = "1.15.*"`。現在のゲーム版との互換性は未検証。
- 原本の中には `.idea` と `.claude/settings.local.json` など開発環境ファイルも含まれる。**旧ZIPを公開リポジトリへそのまま登録しない。** 原本は検証用として所有者が保管する。

## 発見した構造と思想

| 旧サブシステム・主な実在ファイル | 実装上の特徴（静的確認） | 採取する設計思想 | 結論 |
|---|---|---|---|
| `common/scripted_effects/KE_grievance_effects.txt`、`KE_opinion_modifiers.txt`（実際は `common/opinion_modifiers/KE_opinion_modifiers.txt`）、`common/decisions/KE_reconciliation_decisions.txt` | 二国間の怨恨 `KE_grievance@TAG`、戦争被害 `KE_war_damage@TAG`、和解 `KE_reconciliation@TAG`、直近緊張 `KE_recent_tension@TAG`。違う減衰率・係争領土・和解の影響を組み合わせる | 短期の事件と長期の歴史的記憶を区別。二国間の関係には複数の要因がある | **思想を強く継承**、保存構造と更新方法は再設計 |
| `common/decisions/KE_provocation_decisions.txt`、`events/KE_provocation_events.txt` | 国境事件、領空侵犯などの段階的挑発と外交反応 | 危機の増幅/抑制が外交へ連鎖 | 採用候補。対立の原因と解決経路を明示 |
| `common/scripted_effects/KE_autoresearch_effects.txt`、`common/decisions/KE_autoresearch_decisions.txt` | 月次で「研究枠数×30」を独自ポイントに積算、閾値を超えると `set_technology` で技術付与。ON/OFFで進捗変数を残す。研究先は長い固定優先順位 | プレイヤー研究の自動・手動切替。ポイントの端数繰越 | **目的を継承し方式を刷新**。標準研究枠の自動投入を意味しない |
| `common/scripted_effects/KE_development_effects.txt` | 世界発展水準、国家発展指標、陸空海の近代化を三層化。産業/戦争/訓練で変動 | 自然な国家発展と専門化、小国の成長 | 概念を継承、指標の責務と重複を整理 |
| `common/scripted_effects/KE_order_effects.txt`、`events/KE_order_events.txt`、`common/scripted_gui/KE_order_panel.txt` | 大戦後の国際秩序、国際会議、常任・非常任の理事国、投票、制裁、超国家化の段階。決議履歴の直近5件相当の枠を初期化 | 国際秩序が戦争の帰結によって形成/変容する。結果履歴を残す | 国際連盟/戦後制度を区別したイベント駆動の世界秩序に |
| `common/scripted_effects/KE_peace_effects.txt`、`common/decisions/KE_peace_decisions.txt` | 戦争疲弊、和平希望、当事者の交渉力、停戦・休戦・条件付き和平 | 戦争を「完全征服か降伏か」だけにしない | 採用候補。多国間戦争の安全な状態遷移を再設計 |
| `common/scripted_effects/KE_faction_effects.txt`、`events/KE_faction_events.txt`、`common/ai_strategy/KE_ai_faction.txt` | 盟主が前線/自国防衛/予備兵力/空海軍の行動方針を発し、相手が受諾・拒否 | 国家/組織が指針によって自律行動する | 選択的自動化の参考。ただし師団・将軍AIの実現性証明ではない |
| `common/scripted_effects/KE_scripted_effects.txt`、`common/ideas/KE_peacetime_laws.txt`、`common/ai_strategy/KE_ai_peacetime.txt` | 平時の陸空海軍備上限、国力差、戦時動員/予備役。低性能/小国への更新間引き | 部隊過剰なスパムを抑え、軍備の現実性とFPSを両立 | 採用候補。小国への強制的弱体化と戦争不能リスクを検証 |
| `common/decisions/KE_integration_decisions.txt`、`events/KE_integration_events.txt` | 統合、抵抗低下、完全な中核州化の段階 | 占領地域の政治・社会的統合には時間が必要 | 新地図/州制度と一緒に後回しで検証 |
| `common/scripted_effects/KE_sports_effects.txt`、`events/KE_sports_wc_events.txt`、`events/KE_sports_wbc_events.txt` | WC/WBCの仮想大会・開催国・優勝・外交的影響 | 非軍事的な世界史や文化イベントが長期プレイを彩る | **別の任意拡張候補**。コア外交処理へ密結合させない |

## ソースから直接確認できた重要な設計課題

### LEG-RISK-001: Scripted Effect名の重複（静的に確定）
- `common/scripted_effects/KE_order_effects.txt:195` に `KE_calculate_war_exhaustion = {`。
- `common/scripted_effects/KE_peace_effects.txt:8` に同名の `KE_calculate_war_exhaustion = {`。
- 両者は世界規模と国家単位の疲弊を扱い責務が違う。Paradoxパーサーの採用順/上書き/警告の挙動は実機未検証だが、**同一名前空間の衝突可能性がある**。新設計では `KE_world_war_exhaustion_update` と `KE_country_war_exhaustion_update` 等の明確な命名を採用する。

### LEG-RISK-002: 「四半期」と実際のカウンター周期の乖離（静的に確定）
- `common/on_actions/KE_on_actions.txt:181-191,220-257`。
- `counter < 3` の場合は +1、そうでなければ処理して 0 にする構造は、初回オフセット後は**4回の月次呼出しごと**に実行する。「quarterly（3か月ごと）」の説明と異なる。
- 新設計では周期仕様・初回タイミング・処理回数を簡単なシミュレーションテストで一致させる。

### LEG-RISK-003: 自動研究の柔軟性と仕様説明
- `common/scripted_effects/KE_autoresearch_effects.txt:1-55,72-817`。
- 研究枠は最低1・最大5で近似し、研究枠×30ポイントを月次積立。長大なif/elseで技術を一つ選ぶため、方針別研究や新技術追加が面倒。
- コメントは「手動研究をブロック」と述べる一方、`common/ideas/KE_autoresearch_ideas.txt` は「research speed spirits removed」のコメントだけ。**旧コメントと実装の乖離を疑う**。手動研究の実際の挙動は未テスト。
- `set_technology` は「標準研究枠へ新研究を投入」する命令ではない。IronAsiaでは、技術付与による代替進捗/研究選択UI/ゲームの標準研究の三者を明確に区別し、二重取得や資源増殖を防ぐ。

### LEG-RISK-004: 史実時代との不整合
- `common/on_actions/KE_on_actions.txt:393-429`: 国際秩序が未成立なら5年以上の経過で「戦後」国際会議を代替発火する構造。
- 1936年開始時の国際連盟と、第二次大戦後の国連等を別制度で扱いたいIronAsiaには直輸入不適合。起動条件は年数でなく、実際の世界情勢・既存条約・戦争終結を参照。

### LEG-RISK-005: 将来への堅牢性・継続性
- `common/scripted_effects/KE_grievance_effects.txt:407+`: FRA/GER、GER/POL/SOV、CHI/JAP等を条件分岐で初期化。出発点としては有用だが、地図・時代・国の増減や史実分岐に拡張しにくい。
- `common/scripted_effects/KE_development_effects.txt:32-82`: 世界技術水準の産業指標を米国または代替大国に依存。世界水準の代理指標の妥当性要検証。
- `common/on_actions/KE_on_actions.txt` には `on_annex` や `on_capitulate` 専用のクリーンアップ定義が見つからなかった。国家消滅時の外交案件・議決・参照が全処理経路で安全かを確認する必要がある（存在しない＝必ず故障する、とは断言しない）。
- Scripted Effects、イベント、AI戦略、decisions、YML、Python生成器が相互参照。単純なファイルコピーでは新設計に不要な結合が残る。

## 新リポジトリへの移植方針
1. **思想を読む**: 履歴と因果、差異ある国家関係、段階的外交、研究/軍の省力化、長期的世界進展。
2. **一旦分解**: 世界状態・個別事件・方針・実行効果・UI・履歴を独立した責務に分ける。
3. **仕様を再設計**: `docs/62-LEGACY-TO-IRONASIA-MIGRATION.md` を参照。
4. **必要最小POC**: 旧関数/変数をコピーせず、一つの領土係争と外交関係、研究自動化の設計、手動制御を実証。
5. **第三者や個人のデータを無断同梱しない**: 利用者提供ZIPの原本、IDE設定、生成器、画像等はこの段階ではIronAsiaへ再配布しない。

## 証拠水準
- **確認済み**: ZIP内ファイル、記述されている条件分岐と変数、名前重複、計算周期。
- **未検証**: Paradox DSLの現行版での構文受理、イベント発火と効果、セーブ・ロード、AIの反応、負荷、現行DLC互換、既知不具合の再現。
- **提案**: IronAsiaの新スキーマ、責務分離、POC採用方針。元エンジンに実装されていた事実と混同しない。
