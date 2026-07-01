# App Store Connect 登録指示書

この指示書は、別の AI または作業者が `写っちゃダメ` v0.1.0 を App Store Connect に登録し、TestFlight / App Review 提出まで進めるためのものです。

## 重要な前提

- 公式手順は Apple の最新画面を優先すること。
- App Store Connect の作業には `Account Holder`、`Admin`、または `App Manager` 相当の権限が必要。
- Apple の最新契約が未署名の場合、アプリレコードを作成できない可能性がある。
- 旧 Bundle ID `app.outofshot.game` は Apple 側で利用不可だったため使わない。

公式参照:

- App record 作成: https://developer.apple.com/help/app-store-connect/create-an-app-record/add-a-new-app/
- Build upload: https://developer.apple.com/help/app-store-connect/manage-builds/upload-builds/
- App Privacy: https://developer.apple.com/help/app-store-connect/manage-app-information/manage-app-privacy/

## 確定済みの登録値

| 項目 | 値 |
| --- | --- |
| アプリ名 | 写っちゃダメ |
| Primary Language | Japanese |
| Platform | iOS |
| Bundle ID | `jp.co.4dsystem.outofshot` |
| SKU | `out-of-shot-ios` 推奨 |
| Version | `0.1.0` |
| Build | `1` |
| Team ID | `52VR4955FN` |
| 価格 | 無料を想定 |
| カテゴリ | Games を想定 |

## 生成済みビルド

アップロード対象:

```text
build/release/ios/app.ipa
```

確認済み:

```text
CFBundleDisplayName = 写っちゃダメ
CFBundleIdentifier = jp.co.4dsystem.outofshot
CFBundleShortVersionString = 0.1.0
CFBundleVersion = 1
Export = EXPORT SUCCEEDED
```

## App Store Connect でのアプリ作成

1. App Store Connect にログインする。
2. `Apps` を開く。
3. 左上の `+` から `New App` を選ぶ。
4. 次を入力する。
   - Platforms: `iOS`
   - Name: `写っちゃダメ`
   - Primary Language: `Japanese`
   - Bundle ID: `jp.co.4dsystem.outofshot`
   - SKU: `out-of-shot-ios`
   - User Access: 特に制限がなければ `Full Access`
5. `Create` を押す。

Bundle ID が選択肢に出ない場合:

- Apple Developer の `Certificates, IDs & Profiles` で App ID `jp.co.4dsystem.outofshot` が存在するか確認する。
- 存在しない場合は Explicit App ID として作成する。
- Capabilities は現状、特別なものを追加しない。Camera / Photos は Info.plist の権限文言で扱う。

## IPA アップロード

推奨は Transporter。

1. Mac の Transporter を開く。
2. `build/release/ios/app.ipa` を追加する。
3. Apple ID / App Store Connect アカウントでログインする。
4. `Deliver` を実行する。
5. アップロード完了後、App Store Connect の対象アプリで build `0.1.0 (1)` が処理完了になるまで待つ。

代替手段:

- Xcode Organizer から Archive を選んで distribute する。
- `xcrun altool` を使う場合は Apple ID と app-specific password または適切な認証情報が必要。

## スクリーンショット / 画像

iOS 用画像:

```text
store-assets/ios/ja-JP/iphone-6-9/
store-assets/ios/ja-JP/ipad-13/
store-assets/ios/ja-JP/iphone-6-9-preview.png
```

アップロード方針:

- iPhone 6.9 inch 用に `store-assets/ios/ja-JP/iphone-6-9/01-hero.png` から `06-settings.png` を登録する。
- iPad 対応が有効なため、iPad 13 inch 用に `store-assets/ios/ja-JP/ipad-13/01-hero.png` から `06-settings.png` も登録する。
- App Preview 動画は未作成。必須でなければ登録しない。

## メタデータ草案

### Subtitle

```text
カメラから逃げるパーティーゲーム
```

### Promotional Text

```text
スマホを向ける人と逃げる人に分かれて、撮られない瞬間を楽しむカメラゲームです。
```

### Description

```text
写っちゃダメは、スマホのカメラを使って遊ぶパーティーゲームです。

アウトカメラでは、鬼が目隠しをしてシャッターを切り、他のプレイヤーは写真に写らないように逃げます。体が半分以上写ってしまったら負けです。

インカメラでは、画面内を動くフレームから顔をそらして逃げます。撮れた瞬間をあとから選んで保存できます。

短い時間で遊べるので、友だち同士や家族で気軽に盛り上がれます。
```

### Keywords

```text
パーティーゲーム,カメラ,写真,鬼ごっこ,ミニゲーム,友達,家族
```

### Support URL / Marketing URL / Privacy Policy URL

未確定。所有者に確認すること。App Review 提出前に Privacy Policy URL は用意するのが安全。

## プライバシー回答の下書き

実装から確認できること:

- カメラを使う。
- 撮影した写真をユーザー操作で写真ライブラリへ保存する。
- 設定値は端末内の AsyncStorage に保存する。
- アカウント作成、広告、外部 analytics、外部 API 通信は現状見当たらない。
- 写真や顔情報をサーバーへ送信する実装は現状見当たらない。

App Privacy の想定:

- データ収集: `No, we do not collect data from this app` を想定。
- ただし、所有者が別途 analytics、広告、クラッシュレポート、問い合わせフォーム等を導入している場合は必ず見直す。

権限文言:

```text
NSCameraUsageDescription = ゲーム中に写真を撮影するためカメラを使用します。
NSPhotoLibraryAddUsageDescription = 撮影した写真をカメラロールへ保存するため写真ライブラリを使用します。
```

## 年齢制限 / コンテンツ

想定:

- 暴力、性的表現、ギャンブル、ユーザー生成コンテンツ、Web アクセスなし。
- カメラを使って人を撮影するゲームなので、実際の質問項目に沿って正直に回答する。

最終的な年齢レーティングは App Store Connect の質問結果を採用すること。

## 暗号化 / 輸出コンプライアンス

このアプリ独自の暗号化機能は現状見当たらない。App Store Connect の輸出コンプライアンス質問では、標準 OS / HTTPS 等のみかどうかを実際の設問に沿って確認して回答すること。

## 提出前チェックリスト

- App record が `jp.co.4dsystem.outofshot` で作成されている。
- `build/release/ios/app.ipa` をアップロード済み。
- App Store Connect で build `0.1.0 (1)` の処理が完了している。
- iPhone / iPad スクリーンショットを登録済み。
- Subtitle、Description、Keywords を登録済み。
- Support URL、Privacy Policy URL を登録済み。
- App Privacy を回答済み。
- Age Rating を回答済み。
- Pricing and Availability を無料で設定済み。
- App Review Information に連絡先を入力済み。
- サインイン不要アプリとして、レビュー用アカウントは不要と説明している。

## レビュー向けメモ案

```text
このアプリはログイン不要です。
カメラ権限を許可すると、アウトカメラ / インカメラの2種類のカメラゲームを遊べます。
撮影した写真はユーザーが保存操作をした場合のみ端末の写真ライブラリへ保存されます。
写真や顔情報を外部サーバーへ送信する機能はありません。
```
