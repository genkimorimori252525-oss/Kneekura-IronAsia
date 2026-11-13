# Kneekura-IronAsia

> **手軽に、深く。世界は自ら動く。**
>
> HOI4を長く遊ぶための、自動化可能で、歴史の因果とプレイヤーの選択に応える統合MOD。2026-11-11からのゼロベース再設計。

## 現在の状態
**設計・技術調査段階（実ゲームMODは未実装）**。
現在のHOI4版/DLCは未固定。仕様書の提案は動作・セーブ互換を保証しない。研究の自動投入、戦闘群単位の独立AI、外部JSON出力は特に事前実証が必要。

## まず読む資料
1. [00 設計理念・可変操作・ユーザー体験](docs/00-VISION-AND-PRINCIPLES.md)
2. [10 参考MOD/OSSの研究アトラス（出典・教訓）](docs/10-RESEARCH-ATLAS.md)
3. [11 既存MODの実装手法・失敗例のケーススタディ](docs/11-IMPLEMENTATION-CASE-STUDIES.md)
4. [12 Reddit等での長期プレイ評価と永続化機能の調査](docs/12-LONGEVITY-COMMUNITY-RESEARCH-2026.md)
5. [20 世界・外交・研究・戦闘群・海軍・地図の構造案](docs/20-SYSTEM-ARCHITECTURE.md)
6. [21 史実の国際連盟・国連制度と時代別の理事会構成](docs/21-HISTORICAL-INSTITUTIONS.md)
7. [22 動的な国際秩序・常任/非常任議席と改革の設計](docs/22-DYNAMIC-WORLD-ORDER-DESIGN.md)
8. [24 技術力システムのゼロベース再設計](docs/24-TECHNOLOGICAL-CAPABILITY-REFOUNDATION.md)
9. [25 研究自動化・技術伝播・戦後技術の統合設計](docs/25-RESEARCH-POLICY-AND-LONG-HORIZON.md)
10. [26 技術力システムの実証・バランス試験](docs/26-TECHNOLOGY-EXPERIMENT-AND-BALANCE.md)
11. [27 技術研究後の産業・装備更新・国際標準化アイデア](docs/27-TECH-ADOPTION-AND-INDUSTRIAL-CONTINUITY-IDEAS.md)
12. [28 人材・平時経済・軍組織の変化と国家の盛衰](docs/28-HUMAN-CAPITAL-PEACETIME-AND-INSTITUTIONAL-CHANGE.md)
13. [29 WW2の戦略的な面白さを戦後に続けるキャンペーン循環](docs/29-WARTIME-ENGAGEMENT-LONG-CAMPAIGN-LOOP.md)
14. [30 実現性マトリクス・POC・失敗時の扱い](docs/30-FEASIBILITY-RISKS.md)
15. [31 史実から抽出する外交・同盟・危機・経済の因果モデル](docs/31-HISTORICAL-CAUSAL-ATLAS.md)
16. [40 小さく実証する開発ロードマップ](docs/40-ROADMAP.md)
17. [50 セーブ・歴史・外部JSONのデータ契約](docs/50-DATA-AND-HISTORY.md)
18. [60 旧にーくらエンジンの原本確認と思想継承](docs/60-LEGACY-ENGINE-IMPORT.md)
19. [61 所有者提供ZIPの静的監査・具体的な設計上の問題](docs/61-LEGACY-ARCHIVE-AUDIT.md)
20. [62 旧思想を新IronAsiaへ引き継ぐ再設計計画](docs/62-LEGACY-TO-IRONASIA-MIGRATION.md)

## 旧KneekuraEngineの確認
2026-11-12にユーザー提供の `KneekuraEngine.zip` を受領・静的解析。旧MODはHOI4 1.15系向けと記載され、歴史的怨恨/和解、国際秩序、和平交渉、技術開発、自動研究、平時軍備や同盟指令などを実装しようとしていた。

**思想のみを新規再設計に継承**し、旧ZIP・個人設定・旧コードはリポに同梱しない。重複定義や更新間隔の不整合など静的に確認した課題は [61監査](docs/61-LEGACY-ARCHIVE-AUDIT.md) に整理した。現行版でのゲーム動作は未検証。

## 国際機関の動的制度改革（2026-11-14に追加）
国際連盟と国際連合を別機関・別憲章として扱い、任期・非常任選挙・代表権・国家継続・常任議席の憲章改正を分離。各機関の仲裁、制裁、停戦、復興や現実の履行負担を再現する。史実では安保理常任席は国力ランキングで入れ替わらない点に注意。詳しくは[21史実基線](docs/21-HISTORICAL-INSTITUTIONS.md)と[22動的設計](docs/22-DYNAMIC-WORLD-ORDER-DESIGN.md)を参照。現時点は計画のみ。

## 長期プレイとコミュニティ調査（2026-11-14）
Redditや公式2025年陣営改革を踏まえ、[長期プレイ調査](docs/12-LONGEVITY-COMMUNITY-RESEARCH-2026.md)で「陣営が理由を持って結束・改革・脱退する」「戦争を必ず全面降伏まで続けない」「平時軍縮・再軍備」「国家方針枯渇後の継続課題」「地域別脅威」「自動化」を整理。**既存の陣営ルール等を無駄に再実装しない。実装可否はPOC待ち。**

## 技術力/研究のゼロベース設計（2026-11-14追加）
旧エンジンの「世界floor・国家発展0〜100・陸海空近代化0〜100・固定自動研究」を無条件移植しない。新しい[24 技術力](docs/24-TECHNOLOGICAL-CAPABILITY-REFOUNDATION.md)、[25 研究方針・技術伝播](docs/25-RESEARCH-POLICY-AND-LONG-HORIZON.md)、[26 POC](docs/26-TECHNOLOGY-EXPERIMENT-AND-BALANCE.md)を正本とする。**世界最先端・一般化した知識・国別研究能力・生産への普及・実戦運用を区別**し、研究枠の少ない国にも専門化の道を残す。まだ実ゲーム未実装。

## 人材・平時の国家発展（2026-11-14追加候補）
[28 人材形成・民需経済・組織文化と国家の盛衰](docs/28-HUMAN-CAPITAL-PEACETIME-AND-INSTITUTIONAL-CHANGE.md)を調査候補として追加。技術研究と工業/人材をつなぎ、軍需一辺倒ではない平時発展を考える。ただし既存ドクトリン熟練度やParadox検討中の新機構との重複を避ける。未実装。

## WW2の緊迫感を戦後にも継ぐ設計（2026-11-14追加）
[29 戦略的な競争と戦争品質の継続ループ](docs/29-WARTIME-ENGAGEMENT-LONG-CAMPAIGN-LOOP.md)を追加。世界大戦を日付で定期発生させるのではなく、利害対立→外交/再軍備→危機→必要なら戦争→講和/復興→新秩序をゲーム状態から生成。敵AIの補給/生産/戦域の意思決定も重視し、無料のAI工場・師団を追加して難易度を偽装しない。現段階は設計・実機未検証。

## 史実の出来事でなく因果をモデル化（2026-11-14）
[31 Historical Causal Atlas](docs/31-HISTORICAL-CAUSAL-ATLAS.md) に1919–1991年の歴史的経路（連盟の成否、戦後の同盟再編、非同盟、スエズ危機、キューバ危機、米中関係、資源禁輸、脱植民地化、ソ連崩壊）を出典つきで分析。**強制年代イベント・戦争件数ノルマ・プレイヤーに応じた人工敵生成は禁止**。複数の反実仮想で検証する。

## 重点テーマ
- 国際連盟、戦後の国際連合など時代に沿った制度・国際秩序の変化
- 領土問題、歴史的対立、緊張、外交関係の改善と悪化、仲裁、講和
- 研究/軍/外交の**分野別・戦闘群別に選べる自動化と手動化**
- 海軍DLCの複雑さと研究量を整理。小国にも無理のない技術進展
- アフリカ・中国等のプロヴィンス/州分割を歴史と戦略価値で改良
- 終戦後も動く世界と、重要な歴史的変化の追跡

## 開発者・AI向けの約束
- 旧コードの移植を目的にせず、確認済みの理念を再評価して設計する。
- 外部MODやツールを全部入れない。出典・実装根拠・代案・ライセンスを確認して選ぶ。
- ゲームを実際に動かすまで「実装済み」「完全自動化できる」と言わない。
- バニラファイルは直接編集せず、MODと開発資料は独立管理する。
- 開発AIは重複を減らし、設計の改善を提案してよい。ユーザー要件を根拠なしに削除しない。
- 新規開発者は [30-FEASIBILITY-RISKS](docs/30-FEASIBILITY-RISKS.md) と [40-ROADMAP](docs/40-ROADMAP.md) を確認してから実装する。

## 注意
現時点は設計と資料整備のみ。既存MODの素材・コードの無断流用はしない。第三者の作品へのリンクは分析用であり、本MODへの同梱や再配布の許可を意味しない。
