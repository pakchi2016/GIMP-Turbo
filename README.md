# GIMP-Turbo 開発計画

## 概要
**URL:** chatGPT 要件定義キャンバス

## 目的
画像品質向上に特化したソフトウェアの開発

## 経緯
画像生成AIを利用した画像・イラスト生成を行う中で、画像の加工を行うためには一旦画像の高画質化が必要となっていた。しかし、高画質化には手間のかかるソフトウェアや、有料の手順が多いため、簡単で費用のかからない自分用のソフトウェア開発を決断。

## プロジェクト方針
1. **「まずは完成させろ」が最優先課題**
2. **再現性の高い手順で開発**
3. **明確かつ詳細なドキュメントが存在する手順を選択**

## 要件定義
### 開発要件
1. 画像処理機能を持つコンソールアプリの開発
2. ユーザーインプットおよびアウトプット専用のGUIアプリの開発
3. コンソールアプリとGUIアプリの統合

### 技術要件
1. 再現性の高いライブラリ（作業手順が明確）
2. 実装が容易なライブラリ（作業ミスが起こりづらい）
3. 処理速度が期待できるライブラリ（性能要件1）
4. 拡張機能が期待できるライブラリ（性能要件2）

### 機能要件
#### コンソールアプリ
- 画像解析
- 画像品質向上

#### GUI機能
- 画像取り込み
- 画像保存
- 画像表示
- プレビュー表示
- パラメータ調整

## 詳細設計
### コンソール設計
- **ノイズ解析:** 取り込んだ画像のノイズを解析し、適切なパラメータを取得
- **ノイズ除去:** 解析結果に従いノイズを除去
- **アップスケーリング:** 解像度を向上させつつ画質低下を抑制
- **画像解析:** 画質向上に必要なパラメータを取得
- **画質向上:** 解析結果に従い画質向上処理を実施
- **解像度復元:** アップスケーリング前のサイズに復元
- **パラメータ引き渡し:** 解析で得たパラメータをGUIの初期設定に適用
- **プレビュー画像引き渡し:** 処理後のプレビュー画像をGUIに提供

### GUI設計
#### 画像表示
- **取り込み画像:** 左上300x300に表示（「取り込み画像」ラベル付き）
- **プレビュー画像:** 右上300x300に表示（「プレビュー画像」ラベル付き）

#### インジケータ
- **機能:** プレビュー画像の生成進捗を表示
- **配置:** 取り込み画像とプレビュー画像の下

#### パラメータ調整
- **機能:** 画像処理のパラメータ調整スライダー
- **スライダー種類:**
  - ノイズ除去
  - アップスケーリング
  - 画像品質向上（細分化要検討）

#### ボタン
- **画像取り込み:** ダイアログボックスで画像を取り込む（スライダー下左側）
- **画像保存:** プレビュー画像を保存（スライダー下右側）
- **キャンセル:** 画面を閉じる（右下端）

## コンポーネント設計
### 主要ライブラリ
- **画像解析ライブラリ:** TBD
- **画像処理ライブラリ:** TBD
- **GUIライブラリ:** PyQt

## 画像処理設計
- **ノイズ解析方式:** TBD
- **ノイズ除去方式:** TBD
- **アップスケーリング方式:** TBD
- **画質解析方式:** TBD
- **画質向上方式:** TBD

## 開発環境
### 基盤
- **WSL + Ubuntu + Docker**（安定した環境を優先）
- **Docker イメージ:** Ubuntu

### バージョン管理
- **Git + GitHub**

## フォルダ構成（Windows）
```
D:/dev/GIMP-Turbo/
│── docker/             # Docker 環境
│── product/            # GitHub リポジトリ (ルート)
│   ├── Step1/          # 各開発ステップのプロジェクト
│   ├── Step2/
│   ├── Step3/
│   ├── Step4/
│   ├── Step5/
│   ├── docs/           # ドキュメント
│   ├── README.md
│   ├── LICENSE
│   └── .gitignore
```

## マイルストーン（Step 0～7）
### Step 0: 環境構築
- 開発環境の構築
- 必要なライブラリを導入

### Step 1: コンソールアプリの基本動作構築
- 画像の入出力を実装（ハードコーディング）
- 最小限のエラー処理（ログ出力）

### Step 2: 右クリック「送る」メニュー連携
- PowerShellスクリプトを使用し、SJISからUTF-8へ変換
```powershell
param([string]$filePath)
$utf8Path = [System.Text.Encoding]::UTF8.GetString(
    [System.Text.Encoding]::GetEncoding("Shift_JIS").GetBytes($filePath)
)
wsl ./my_console_app "$utf8Path"
pause
```

### Step 3: 簡易画像処理
- 固定パラメータで画像処理を実施

### Step 4: 自動解析と画像処理の初期実装
- 画像の解析を実施し、最適化

### Step 5: GUI 開発
- 画像処理コンソールとの統合を前提にGUIを開発

### Step 6: コンソールとGUI の連携
- コンソールとGUIを統合し、スタティックリンク化

### Step 7: ソフトウェアのブラッシュアップ
- **動作安定性向上:** 不安定箇所の改善
- **UI/UX改善:** 使いやすさを考慮した改良
- **処理最適化:** 高速化と正確なアルゴリズムの導入
