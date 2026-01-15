# Cornix用ZMKキーボード

CornixはRMKですが、ソースコード非公開です。このフォークしたコミュニティファームウェアは、CornixをZMKで動作するようにしています。
> ZMKの分割ガイドラインに従った完全な左右役割設定、バッテリー電源管理、およびBluetoothセントラル/ペリフェラル設定を提供します。

また、eswaiさん作成のZMK版薙刀式をフォークし、改造した、[wataruw/zmk-naginata](https://github.com/wataruw/zmk-naginata)をリンクすることで、Cornixで薙刀式かな入力を実現します。

> 薙刀式は大岡俊彦さんが考案なされた、かな入力方式です。  
> [カナ配列【薙刀式】(カタナ式ファミリー)](http://oookaworks.seesaa.net/article/456099128.html#gsc.tab=0)

## RGBについて（フォーク元のままの日本語訳）

Cornixシールドには片側に2つのRGB LEDがあり、標準ファームウェアではPWMで制御されています。

これに代わる解決策として、RGBインジケーターモジュールを適応させてこれらのRGBを点灯させ、標準ファームウェアと同様の効果（バッテリー残量や接続状態を示すためにRGB LEDを使用する）を実現することです。

しかし、このリポジトリではまだサポートされていません。PR歓迎です！

※フォーク元で対応したら、取り込めるかもしれません。

## 対応ハードウェア：Cornix分割キーボード（フォーク元のままの日本語訳）

Cornix Split Tented Low‑Profile Ergo Keyboard (Jezail Funder)

Cornixは、Corneにインスパイアされた分割エルゴノミクスキーボードで、コンパクトな3x6カラムスタッガード配列と6つのサムクラスターキー（片側3つ）を備えています。10°、18°、25°の調整可能なテンティング角度を提供し、ユーザーの手首の負担を軽減し、独自のエルゴノミクス配置を見つけることができます。

- **分割、カラムスタッガード配列** (3x6 + サムクラスターレイアウト)。
- **調整可能なテンティングサポート** 10°、18°、25° (ハードウェアベース、ファームウェアハックなし)。
- **Kailh Choc V2ホットスワップソケット** およびLAKまたはLCKロープロファイルキーキャップのサポート。
- **デュアルモード接続**: 有線USB-CまたはBluetoothワイヤレス（左側がマスター）。
- **ファームウェア**: キーマップとレイヤーのカスタマイズにVIALを完全サポート、標準ファームウェアはRMKです。
- プレミアムな **CNC加工アルミニウムシャーシ**、カスタムダンピングフォーム、およびポータブル収納ポーチ。

> このプロジェクトの所有者はRMKのコントリビューターでもあります。RMKのサポートもお願いします <https://rmk.rs/>

## --ブートローダー復旧手順--

オリジナルのRMKはSoftDeviceを削除していました。v2.3以前では、`zmk.uf2` をフラッシュする前にSoftDeviceを復元する必要があったようです（bootloader/README.mdを参照）

v2.3以降は、フラッシュパーティションが更新（SDの縮小/削除）されたため、ファームウェアを直接フラッシュできます。

v2.3以降では、通常、`cornix_left_default.uf2` と `cornix_right.uf2` をフラッシュする前にreset.uf2は**不要**です。reset.uf2を使用するのは以下の場合のみです：

- SDに依存していたv2.3以前のレイアウトからアップグレードする場合
- ブートローダー/パーティションが破損しており、UF2が期待通りに動作しない場合

> 以前のバージョンからのファームウェアリセットにはreset.uf2が必要になる場合があります。
> 元のuf2ファイルをフラッシュすることで標準ファームウェアにロールバックできます（rmkファームウェア/配下のファイルをバックアップしてください）。

## 🔰 簡単な方法：このリポジトリをクローンしてGitHub Actionsでビルドする

ZMKを初めて使用し、`west.yml` やモジュール管理を行いたくない場合は、このリポジトリを直接使用してファームウェアをカスタマイズできます。

### 手順

1. **このリポジトリをフォークまたはクローンする**
   - 右上の **Fork** をクリックしてこのリポジトリをあなたのGitHubアカウントにコピーするか、
   - ローカルで `git clone` を実行します。

   > フォークをお勧めします。GitHub Actionsが自動的にファームウェアをビルドするからです。

2. **キーマップを編集する**
   - `config/cornix.keymap` にあるキーマップファイル（またはカスタマイズしたい `.keymap` ファイル）を見つけます。
   - 直接編集するか、[ZMK Keymap Editor](https://nickcoutsos.github.io/keymap-editor/) を使用できます：
     - エディタを開き、`.keymap` ファイルを読み込みます。
     - ビジュアルエディタで変更を行います。
     - 更新されたファイルをダウンロードし、リポジトリ内のものと置き換えます。
     - 変更をコミットしてGitHubにプッシュします。

3. **GitHub Actionsでビルドする**
   - プッシュ後、GitHub Actionsが自動的にビルドを実行します。
   - ワークフローが完了したら、**Actions → 最新の実行 → Artifacts** に移動し、ファームウェア（`.uf2`）ファイルをダウンロードします。

4. **キーボードをフラッシュする**
   - ボードをUF2ブートローダーモードにします（通常はリセットボタンをダブルタップします）。
   - マウントされたドライブに `.uf2` ファイルをドラッグアンドドロップします。

## 🧩 ZMK Studioサポート（フォーク元のままの日本語訳：未検証）

このリポジトリは、左側/中央側でZMK Studio用に設定されています。

- ビルドには、左側のみに `studio-rpc-usb-uart` スニペットと `CONFIG_ZMK_STUDIO=y` が含まれています。
- ZMK Studio内で使用するために予約済みレイヤー（`extra1`、`extra2`）を追加しました。
- Raiseレイヤーの左上の位置（以前の `&none` を置き換え）に、ロック解除用バインディング `&studio_unlock` が利用可能です。

使い方：

1. GitHub Actionsで生成された左側のファームウェアをビルドしてフラッシュします。
2. USBで接続し、ZMK Studio（Webまたはネイティブアプリ）を開きます。
3. キーボード出力がUSBであることを確認します（必要に応じて `&out OUT_USB` を呼び出します）。
4. `&studio_unlock` キーを1回押して編集を許可します。
5. Studioでキーマップを変更します。注：Studioが制御を引き継ぐと、Studioで「Restore Stock Settings」を実行しない限り、`.keymap` への変更は適用されません。

注意：

- Studioビルドはより多くのRAM/フラッシュを使用します。サイズ制限に達した場合、機能を調整できます。
- 分割キーボードの場合、Studioを有効にする必要があるのは左側（中央）のみです。

### 誰向けですか？

- ZMKの初心者
- キーマップのみをカスタマイズしたいユーザー
- ドライバーやハードウェア定義を変更する必要がない人

## Cornix ZMKファームウェアをスクラッチからビルドする方法（フォーク元のままの日本語訳）

このセクションでは、公式のZMKファームウェア開発プロセスを使用して、Cornix ZMKファームウェアをスクラッチからビルドする方法を説明します。

### 前提条件

開始する前に、以下を確認してください：

- GitHubアカウント
- システムにGitがインストールされていること
- GitとGitHubの基本的な理解
- CornixキーボードPCBの準備

### ステップ1：ZMK設定リポジトリの初期化

1. **新しいリポジトリを作成する** 公式のZMK設定テンプレートを使用：
   - アクセス：<https://github.com/zmkfirmware/unified-zmk-config-template>
   - "Use this template" → "Create a new repository" をクリック
   - リポジトリに名前を付ける（例：`cornix-zmk-config`）
   - "Public" または "Private" を選択
   - "Create repository" をクリック

2. **新しいリポジトリをローカルにクローンする**：

   ```bash
   git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
   cd YOUR_REPO_NAME
   ```

3. **ZMK開発環境を初期化する**：

   ```bash
   west init -l config/
   west update
   west zephyr-export
   ```

> **重要**：ZMKファームウェア開発には学習曲線があるため、進める前にZMKドキュメントをよく読んでおく必要があります。
>
> - ZMKカスタマイズガイド：<https://zmk.dev/docs/customization>
> - ZMK設定：<https://zmk.dev/docs/user-setup>

### ステップ2：プロジェクトへのCornixシールドの追加

zmk-configリポジトリを初期化した後、次のセクションの手順に従ってCornixシールドを統合します。

## 既存のZMKプロジェクトにCornixシールドを追加する方法（フォーク元のままの日本語訳）

既存のzmk-configを持つユーザーの場合、west.yml経由でこのリポジトリの依存関係を追加し、west updateで最新バージョンを取得します：

### 1. west.ymlの修正

`config/west.yml` ファイルを編集し、`manifest/remotes` セクションに追加します：

```yaml
remotes:
  - name: zmkfirmware
    url-base: https://github.com/zmkfirmware
  - name: cornix-shield
    url-base: https://github.com/hitsmaxft
  - name: urob
    url-base: https://github.com/urob
```

`manifest/projects` セクションに追加します：

```yaml
projects:
  - name: zmk
    remote: zmkfirmware
    revision: main
    import: app/west.yml
  - name: zmk-keyboard-cornix
    remote: cornix-shield
    revision: main
  - name: zmk-helpers
    remote: urob
    revision: main
```

### 2. 依存関係の更新

```bash
west update
```

### 3. ビルド設定

`build.yaml` ファイルを編集し、以下を追加します：

> [!NOTE]
>
> 1. ドングルなしで（デフォルトの）Cornixを使用する場合、"cornix_left"、"cornix_right"、"reset" を選択してください。
> 2. ドングル付きでCornixを使用する場合、"cornix_dongle"、"cornix_left_for_dongle"、"cornix_right"、"reset" を選択してください。(このリポジトリではドングルのビルドは消してありますので、生成しません)
> 3. RGB LEDライトを有効にするには "cornix_indicator" シールドを追加してください。消費電力が大幅に増えるため、自己責任で使用してください。

```yaml
include:
  # ドングル付きでCornixを使用
  - board: cornix_dongle
    shield: cornix_dongle_eyelash dongle_display
    snippet: studio-rpc-usb-uart
    artifact-name: cornix_dongle

  - board: cornix_ph_left
    # shield: cornix_indicator
    artifact-name: cornix_left_for_dongle

  # ドングルなしでCornixを使用
  - board: cornix_left
    # shield: cornix_indicator
    artifact-name: cornix_left

  - board: cornix_right
    # shield: cornix_indicator
    artifact-name: cornix_right

  - board: cornix_right
    shield: settings_reset
    artifact-name: reset
```

### 4. ファームウェアのビルド

好みの方法でビルドしてください。

- v2.3以降、SDの復旧は不要です
- Cornixの両側にreset.uf2をフラッシュします
- 左と右のuf2ファイルをフラッシュします
- 両側を同時にリセットします

### 5. ファームウェアのフラッシュ

生成された `.uf2` ファイルを対応するマイクロコントローラーにフラッシュします：

- 左半分：`build/left/zephyr/zmk.uf2`
- 右半分：`build/right/zephyr/zmk.uf2`

## カスタムドングルユーザー向けドングルアダプターシールド（フォーク元のままの日本語訳）

独自のカスタムドングル設定を作成したいユーザーのために、このリポジトリはアダプターシールドを提供します。Cornixドングルの完全な設定では、複数のシールドを使用できます：

1. **`cornix_dongle_adapter`** - マトリックスとBluetooth機能のための共通シールド
2. **`dongle_display`** - ドングル画面（または別のディスプレイプロジェクト）用のディスプレイモジュール
3. **`cornix_dongle_eyelash`** - ボード用のディスプレイデバイスを設定するためのサンプルシールド（ボードがデバイスツリーに既に `zephyr,display` を持っている場合、このディスプレイオーバーレイシールドは不要です）

`build.yaml` ファイルの設定は、eyelashドングル用のシールドの使用方法を示しています：

```yaml
include:
  # Cornixをドングル付きで使用
  - board: nice_nano_v2
    shield: cornix_dongle_adapter cornix_dongle_eyelash dongle_display
    snippet: studio-rpc-usb-uart
    artifact-name: cornix_dongle
```

ディスプレイ部分のカスタムシールドを作成するには：

1. `dongle_display` モジュールはディスプレイウィジェットを含むモジュールであり、west経由またはローカルでプロジェクト依存関係の一部として含まれます。
2. ディスプレイハードウェア用のカスタムシールドを作成する必要がある場合は、適切なディスプレイ設定を提供する新しいシールドを作成できます。ここでは例として `cornix_dongle_eyelash` を示しています。
3. ボードがデバイスツリーに既に `zephyr,display` を持っている場合は、`cornix_dongle_eyelash` シールドを省略できます。
4. ビルド設定にカスタムシールドを含めます。

カスタムドングル画面の場合、カスタムドングル用の新しいターゲットをbuild.yamlに追加します：

```yaml
- board: nice_nano_v2
  shield: cornix_dongle_adapter cornix_dongle_eyelash dongle_display
  snippet: studio-rpc-usb-uart zmk-usb-logging
  artifact-name: cornix_dongle
```

ディスプレイ用のカスタムシールドを作成するには：

1. マトリックスとBluetooth機能のベースシールドとして `cornix_dongle_adapter` を使用します。
2. 適切なボードと設定を含むカスタムシールドを `build.yaml` ファイルに追加します。
3. `cornix_dongle_eyelash` を例として使用し、カスタムボードに合わせてディスプレイ部分を変更します。
4. `cornix_dongle_eyelash` をプロジェクトの `boards/shield/` ディレクトリにコピーし、同じ名前を使用するか、新しいシールドとして名前を変更できます。

`west.yml` ファイルの設定は同じままです：

```yaml
remotes:
  - name: zmkfirmware
    url-base: https://github.com/zmkfirmware
  - name: cornix-shield
    url-base: https://github.com/hitsmaxft
  - name: urob
    url-base: https://github.com/urob
```

```yaml
projects:
  - name: zmk
    remote: zmkfirmware
    revision: main
    import: app/west.yml
  - name: zmk-keyboard-cornix
    remote: cornix-shield
    revision: main
  - name: zmk-helpers
    remote: urob
    revision: main
```

## このプロジェクトをローカルでビルドする (west.yaml依存なし)（フォーク元のままの日本語訳）

west.yamlの依存関係として追加せずにこのプロジェクトをローカルでビルドしたい場合は、ZMK_EXTRA_MODULES cmake引数を使用できます。

### 前提条件 2

1. 動作するZMK開発環境がセットアップされていること
2. このリポジトリをローカルディレクトリにクローンする

### ビルド手順

1. **このリポジトリをクローンする**：

   ```bash
   git clone https://github.com/hitsmaxft/zmk-keyboard-cornix.git
   ```

2. **追加モジュールを使用してZMKビルドを設定する**：

   `.west/config` ファイルを編集し、`[build]` セクションの下にcmake引数を追加します：

   ```ini
   [build]
   cmake-args = -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DZMK_EXTRA_MODULES=/full/absolute/path/to/zmk-keyboard-cornix
   ```

   `/full/absolute/path/to/zmk-keyboard-cornix` を、このリポジトリをクローンした実際の絶対パスに置き換えてください。

3. **ファームウェアをビルドする**：

   ```bash
   west build -b cornix_main_left
   west build -b cornix_right
   ```

この方法を使用すると、既存のZMK設定のwest.yamlファイルを変更せずにCornixシールドを使用できます。
