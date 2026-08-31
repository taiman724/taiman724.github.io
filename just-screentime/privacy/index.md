---
layout: page
title: Privacy Policy
permalink: /just-screentime/privacy/
---

# Just ScreenTime Privacy Policy / プライバシーポリシー

**Last updated / 最終更新:** 2026-08-31
**Applies to / 対象:** Just ScreenTime 1.2.2 (Microsoft Store; other builds are private developer QA only / Microsoft Store、その他のビルドは開発者の非公開QA用のみ)

---

## English

### 1. Summary
Just ScreenTime's installed tracking, reporting, export, and HUD features
operate entirely on the user's Windows PC and do not send app data to an
external service. The only external navigation is the user-initiated Privacy
Policy link, which opens this public page in the default browser. The app
contains no telemetry, analytics, advertising, cloud sync, or remote
crash-reporting SDK. It writes small diagnostic error
logs locally when an exception occurs; those logs are never sent automatically.
The optional Live HUD is off by default, requires valid tracking authorization,
and uses only locally processed tracking data.

### 2. Data collected
Just ScreenTime records the following locally, for the sole purpose of showing the
user how they spend time on their own PC:

- The executable path and process name of the foreground application.
- Per-application start time, end time, and duration, broken down into three
  session types: Active (foreground + user input), Fg-Idle (foreground + user
  idle beyond the configured threshold), and Background (app window open while
  another app is foreground).
- Optional diary posts entered by the user, including the post time and the
  active application name and executable path attached as local context when
  the post is created. If the weekly digest is explicitly enabled (it is off
  by default), the app also generates one local diary summary from the same
  usage records after 18:00 on Sunday.
- User-configurable settings (idle threshold, data retention days, theme,
  shell exclusion, focus-mode allowlist, daily active limit, weekly digest,
  focus-timer preferences, tracking authorization, and Live HUD enabled state,
  transparency, display selection, and normalized drag position).
- Application display metadata derived locally from executable files, including
  names and icons cached for the dashboard and optional Live HUD.
- Optional local export files (CSV/JSON) the user explicitly saves on this PC.
- Local diagnostic error logs created only when an exception occurs. They can
  contain a timestamp, component name, exception message, stack trace, and local
  file or application paths.

It does **not** record window titles, document contents, URLs, keystrokes,
clipboard, screenshots, microphone, camera, or network traffic.

Foreground application identity is obtained locally by the tracking component
only while the current tracking disclosure has been acknowledged and tracking
is enabled. The optional Live HUD reads tracker-written local database state,
usage records, and the application metadata cache to show the foreground app and
its recorded time; it does not independently read process memory or send that
information anywhere.
If tracking authorization is absent, declined, disabled, or becomes invalid, the
HUD cannot be enabled and is stopped fail-closed.

### 3. Where data is stored and protected
- Private unpackaged development/QA build database: `%LOCALAPPDATA%\JustScreenTime\justscreentime.db`
  (SQLite).
- MSIX / Store build database:
  `%LOCALAPPDATA%\Packages\<PackageFamilyName>\LocalCache\Local\JustScreenTime\justscreentime.db`.
- Diagnostic logs: the same per-user `JustScreenTime` LocalApplicationData
  directory, normally package-redirected under `LocalCache` for the Store build.
- Upgrade migration only: if a pre-rename WinTrack database exists, the app
  preserves the original `%LOCALAPPDATA%\WinTrack\wintrack.db`, writes a small
  one-time migration marker beside it, and creates
  `%LOCALAPPDATA%\JustScreenTime\LegacyBackups\wintrack-v1.db`. In the Store
  build, Windows package redirection may place the backup under package-local
  `LocalCache`.
- CSV/JSON exports: the location explicitly selected by the user.

The app relies on Windows user-profile and package-directory access controls. It
does not add separate database encryption, so anyone who can access the user's
Windows profile or backups may also be able to read this local data.

### 4. Data retention
Raw measurement sessions are kept for the user-configured period (7, 30, or 90
days; default 90) and are then deleted by the cleanup service.

Diary posts and settings remain until the user edits or deletes them, uses the
relevant in-app control, deletes the database, or uninstalls the packaged app.
The Settings action for deleting usage data removes measurement sessions,
cached app metadata, and diary posts/notes. It does not reset preferences,
delete diagnostic logs, delete exported CSV/JSON files, or delete a pre-rename
WinTrack source database, migration marker, or migration backup.

Each diagnostic log rolls after approximately 512 KiB and keeps one previous
generation, for approximately 1 MiB per log name. Exported files remain until
the user deletes them from the location where they were saved. A legacy
WinTrack source database and migration backup are not subject to the 7/30/90-day
cleanup; users who no longer need them must delete those files manually.

### 5. Data sharing and user controls
Just ScreenTime does not sell, rent, upload, or disclose app data. The current
version has no server component, account system, cloud sync, telemetry,
analytics, advertising, or remote crash reporting. Windows toast notifications
are generated locally and stay on the device.

Users can change measurement retention, edit or delete diary posts, delete
usage history and diary data from Settings, and choose whether to create an
export. The app never uploads an export. The Live HUD starts off and is shown
only after the user enables it in Settings while tracking authorization remains
valid. Pausing tracking also stops the HUD from refreshing or displaying data.

Selecting the Privacy Policy link opens this public policy website in the
user's default browser. That navigation is user-initiated and is separate from
the app's local data processing. The site is hosted by GitHub Pages; GitHub
states that visitors' IP addresses are logged and stored for security purposes.
The maintainer adds no analytics, advertising, forms, cookies, or tracking
scripts to the policy site and receives no app usage data through it. GitHub's
own privacy terms apply to visits to the site.

Uninstalling the Store package normally removes its package-local database and
logs, including a migration backup if Windows redirected it into package-local
storage. An original pre-rename WinTrack database and other
unpackaged/development data may need to be removed manually. Exported CSV/JSON
files are outside the app package and are not deleted on uninstall. Deleting
only the current SQLite database does not delete separately stored diagnostic
logs, exported files, or legacy migration files.

### 6. Children
Just ScreenTime has no account or online service and does not receive children's
personal information. If a child uses the app, the same local records described
above may be created in that child's or shared Windows profile.

### 7. Changes
Any future change to this policy will be published at the public Privacy Policy
URL used by the Microsoft Store alongside a new app version, and summarized in
the app's release notes.

### 8. Contact
Questions or requests:
**taiman.jp@gmail.com**

---

## 日本語

### 1. 概要
Just ScreenTime のインストール済み計測、レポート、エクスポート、HUD 機能は
ユーザーの Windows PC 内だけで動作し、アプリのデータを外部サービスへ送信しません。
外部への移動は、ユーザーが Privacy Policy リンクを選択して既定ブラウザーでこの公開
ページを開く場合だけです。テレメトリ、アナリティクス、広告、クラウド同期、
外部クラッシュレポート SDK は含みません。例外発生時には
小さな診断ログを PC 内へ保存しますが、自動送信はしません。
任意機能の Live HUD は初期設定がオフで、有効な計測認可がある場合に限り、
端末内で処理された計測データだけを利用します。

### 2. 収集するデータ
Just ScreenTime は、ユーザー自身が PC の使用時間を把握することだけを目的に、以下を
ローカルに記録します:

- フォアグラウンドアプリの実行ファイルパスおよびプロセス名
- アプリごとの開始/終了時刻・継続秒数 (3 種類のセッションに分類:
  Active = 前面かつ操作あり / Fg-Idle = 前面かつアイドル閾値超過 /
  Background = 別アプリが前面・当該アプリのウィンドウは開いている)
- ユーザーが任意に入力する日記本文、投稿時刻、および投稿時の文脈として添付される
  アクティブなアプリ名・実行ファイルパス。週次ダイジェストを明示的に有効化した場合
  （初期設定はオフ）、同じ利用記録から日曜18時以降に1件のローカル日記要約も
  自動生成します
- ユーザー設定 (アイドル閾値、データ保持日数、テーマ、シェル除外、
  フォーカス許可リスト、1日のアクティブ上限、週次ダイジェスト、
  フォーカスタイマー設定、計測認可、Live HUD の有効状態、透過率、表示先、
  およびドラッグ位置の正規化座標)
- 実行ファイルから端末内で取得し、ダッシュボードおよび任意の Live HUD 用に
  キャッシュするアプリ表示名・アイコンなどの表示情報
- ユーザーが明示的にPCへ保存するエクスポートファイル (CSV/JSON)
- 例外発生時だけ作成されるローカル診断ログ（日時、コンポーネント名、
  例外メッセージ、スタックトレース、ローカルのファイル／アプリパスを
  含む場合があります）

次のものは記録しません: **ウィンドウタイトル、文書内容、URL、キー入力、
クリップボード、スクリーンショット、マイク、カメラ、ネットワーク通信**。

フォアグラウンドアプリの識別情報は、現行の計測説明への同意が記録され、計測が
有効な間だけ、計測コンポーネントが端末内で取得します。任意の Live HUD は、
計測コンポーネントがローカルDBへ書き込んだ状態、利用記録、およびアプリ情報
キャッシュを読み、前面アプリと記録済み利用時間を表示します。HUD が独自に
プロセスメモリを読むことや、情報を外部へ送信することはありません。計測認可が
未取得、拒否、無効、または不正な状態になった場合、HUD は有効化できず、
fail-closed で停止します。

### 3. 保存場所と保護
- 開発者の非公開QA用・非パッケージ版 DB: `%LOCALAPPDATA%\JustScreenTime\justscreentime.db` (SQLite)
- MSIX / Store 版 DB:
  `%LOCALAPPDATA%\Packages\<PackageFamilyName>\LocalCache\Local\JustScreenTime\justscreentime.db`
- 診断ログ: 同じユーザー別 LocalApplicationData 内の `JustScreenTime`
  ディレクトリ（Store 版では通常 `LocalCache` 配下へリダイレクト）
- 旧 WinTrack 版からの移行時のみ: 元の
  `%LOCALAPPDATA%\WinTrack\wintrack.db` を残し、同じ場所に小さな移行済み
  マーカーを作成して、
  `%LOCALAPPDATA%\JustScreenTime\LegacyBackups\wintrack-v1.db` にバックアップ
  を作成します。Store 版では Windows のパッケージリダイレクトにより、
  バックアップがパッケージ内の `LocalCache` に置かれる場合があります
- CSV/JSON: ユーザーが保存時に明示的に選んだ場所

Windows のユーザープロファイルおよびパッケージディレクトリのアクセス制御を
利用します。DB にアプリ独自の暗号化は追加していないため、Windows
プロファイルやバックアップへアクセスできる人はデータを読める可能性があります。

### 4. 保持期間
生の計測セッションはユーザー指定の期間（7／30／90 日、既定 90 日）だけ保持し、
それ以降はクリーンアップサービスが削除します。

日記と設定は、ユーザーが編集・削除するか、対応するアプリ内操作、DB 削除、
またはパッケージ版のアンインストールを行うまで保持します。設定画面の
利用データ削除操作は、計測セッション、アプリ情報キャッシュ、日記投稿／メモを
削除します。設定、診断ログ、保存済み CSV/JSON、旧 WinTrack の元DB、
移行済みマーカー、移行バックアップは削除しません。

診断ログは各ファイルがおよそ 512 KiB でローテーションし、1 世代前まで
（ログ名ごとにおよそ 1 MiB）保持します。エクスポートファイルは、ユーザーが
保存先から削除するまで残ります。旧 WinTrack の元DBと移行バックアップは
7／30／90 日の自動削除対象ではなく、不要になった場合はユーザーが手動で
削除する必要があります。

### 5. データ共有とユーザー操作
Just ScreenTime はアプリのデータを販売、貸与、アップロード、第三者提供しません。
現行版にはサーバー、アカウント、クラウド同期、テレメトリ、アナリティクス、
広告、外部クラッシュレポート機能がありません。Windows 通知は端末内で生成します。

保持期間の変更、日記の編集／削除、設定からの利用履歴・日記データ削除、
エクスポートするかどうかと保存先の選択ができます。エクスポートをアプリが
アップロードすることはありません。Live HUD は初期設定がオフで、計測認可が有効な
間にユーザーが設定画面で有効化した場合だけ表示されます。計測を停止すると、HUD の
データ更新と表示も停止します。

Privacy Policy リンクを選択すると、ユーザーの既定ブラウザーでこの公開ポリシー
サイトを開きます。この移動はユーザー操作によるもので、アプリ内のローカルデータ
処理とは別です。サイトは GitHub Pages でホストされ、GitHub はセキュリティ目的で
訪問者の IP アドレスを記録・保存すると説明しています。運営者はこのポリシーサイトへ
独自のアナリティクス、広告、フォーム、Cookie、追跡スクリプトを追加せず、サイトを
通じてアプリ利用データを受け取りません。サイト訪問には GitHub 自身のプライバシー
条件が適用されます。

Store 版のアンインストールでは通常、パッケージ内の DB とログ、および Windows
がパッケージ内へリダイレクトした移行バックアップが削除されます。元の旧
WinTrack DB と非パッケージ／開発版のデータは手動削除が必要な場合があります。
CSV/JSON はアプリ管理外なのでアンインストールでは消えません。現在の SQLite
DB だけを削除しても、別ファイルの診断ログ、エクスポートファイル、旧版移行
ファイルは残ります。

### 6. 児童の個人情報
Just ScreenTime にはアカウントやオンラインサービスがなく、児童の個人情報を
運営者が受け取ることはありません。児童が利用した場合は、上記と同じローカル記録が
児童本人または共有の Windows プロファイル内に作成される場合があります。

### 7. 変更
このポリシーを変更する場合は、新しいアプリバージョンと共に Microsoft Store で
使用する公開 Privacy Policy URL に掲載し、リリースノートに要約を記載します。

### 8. お問い合わせ
**taiman.jp@gmail.com**
