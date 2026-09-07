---
layout: page
title: Privacy Policy
permalink: /just-screentime/privacy/
---

# Retime Privacy Policy / プライバシーポリシー

Retime is the new name of Just ScreenTime. / Retime は Just ScreenTime の新しい名称です。

**Last updated / 最終更新:** 2026-09-07
**Applies to / 対象:** Retime 1.2.8 (Microsoft Store; other builds are private developer QA only / Microsoft Store、その他のビルドは開発者の非公開QA用のみ)

**Version notice / バージョンについて:** The database and diagnostic-log encryption described below starts with version 1.2.8. An older Just ScreenTime installation does not gain this protection merely because this page has been updated; it applies after installing 1.2.8 and successfully migrating the local data. / 以下のDB・診断ログの暗号化は1.2.8からの変更です。このページの更新だけで旧Just ScreenTime版の保存データが暗号化されることはありません。1.2.8のインストールとローカルデータの移行が正常に完了した後に適用されます。

---

## English

### 1. Summary
Retime's installed tracking, reporting, export, and HUD features
operate entirely on the user's Windows PC and do not send app data to an
external service. The app uses Microsoft Store services only to check the app
license and trial expiration, retrieve the localized Store price, and complete
a purchase requested by the user. It does not send usage history, diary text,
settings, exports, or diagnostic logs to Microsoft Store and does not receive
payment-card details. The only other external navigation is the user-initiated
Privacy Policy link, which opens this public page in the default browser. The
app contains no telemetry, analytics, advertising, cloud sync, or remote
crash-reporting SDK. It writes small diagnostic error
logs locally when an exception occurs; those logs are never sent automatically.
The optional Live HUD is off by default, requires valid tracking authorization,
and uses only locally processed tracking data.

### 2. Data collected
Retime records the following locally, for the sole purpose of showing the
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

The app also reads the current license state, trial status and expiration time,
and localized product price from Microsoft Store solely to determine access and
show Store-managed purchase information. These values are not app-usage records
and do not include payment credentials. Microsoft Store, not Retime,
manages the Store account, entitlement, trial, and payment transaction.
Microsoft Store may report a small timestamp rounding difference for a 15-day
trial. The app accepts only a bounded difference, caps effective trial access
at 15 days from the first verified observation, and does not move that deadline
later after a refresh or restart.
After a successful Store check, the app may keep a protected local license cache
containing only the package identity, Full/Trial kind, verification and last-
observed times, and trial expiration. A Full fallback is used for at most 30
days when the Store API is temporarily unavailable; a Trial fallback also ends
at its first effective 15-day deadline and never extends the Store trial.
A separate current-user Windows Credential Locker marker stores only the
package identity, a random cache generation, verification/observation times,
the effective trial expiration, and a revocation flag so an older cache cannot
be reused by itself. It contains no password, Store account identifier, payment
credential, or usage history.
Windows manages Credential Locker and may synchronize it with the user's
Windows/Microsoft account settings; Retime does not transmit it.
A currently valid Store-verified trial remains usable if the cache or marker
cannot be saved, but offline fallback is unavailable without matching protected
evidence.
While a trial is active, the protected last-observed time is updated
periodically and during normal app shutdown using process-monotonic elapsed
time. This does not add any collected category or Store account information.

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
- Protected license-resilience cache: the Store package's LocalState, encrypted
  and authenticated to the current Windows user by Windows Data Protection.
- License anti-replay marker: the current user's Windows Credential Locker.
  This marker contains only the limited security fields described above, not
  usage data or account/payment credentials.
- Upgrade migration only: if a pre-rename WinTrack database exists, the app
  preserves the original `%LOCALAPPDATA%\WinTrack\wintrack.db`, writes a small
  one-time migration marker beside it, and creates
  `%LOCALAPPDATA%\JustScreenTime\LegacyBackups\wintrack-v1.db`. In the Store
  build, Windows package redirection may place the backup under package-local
  `LocalCache`.
- CSV/JSON exports: the location explicitly selected by the user.

The current database and app-owned legacy backup use authenticated
ChaCha20-Poly1305 page encryption, including SQLite journals. Each database has
a random key in an adjacent `.key` file protected by Windows Data Protection
(DPAPI) for the current Windows user. Diagnostic logs are also protected by
DPAPI. Existing plaintext databases are migrated through a verified copy before
an atomic replacement; an interrupted migration is retried without overwriting
the original with an incomplete copy. Old diagnostic logs are protected on
startup or the next write when the files are available.
Keep a database and its matching `.key` file together when backing up, and
restore them under the original Windows account. A lost key or Windows profile
may make a database unrecoverable. Windows profile/directory access controls
also apply; encryption does not prevent access by software running as the same
Windows user or an administrator who controls that account.
The original pre-rename WinTrack source and user-selected CSV/JSON exports keep
their existing, unencrypted formats. Protect those files and older external
backups separately. Temporary files from a plaintext migration are removed
after completion or a successful retry.

### 4. Data retention
Raw measurement sessions are kept for the user-configured period (7, 30, or 90
days; default 90). Eligible sessions are deleted on the cleanup service's next
scheduled run while tracking is running. Cleanup is paused when tracking or
Store access is unavailable and resumes after tracking restarts; stopping
tracking does not delete saved history immediately.

Diary posts and settings remain until the user edits or deletes them, uses the
relevant in-app control, deletes the database, or uninstalls the packaged app.
The protected license cache is replaced after later successful Store checks and
is removed when the packaged app's LocalState is removed. A Full fallback grant
is never valid for more than 30 days from Store verification; a Trial fallback
also ends at the first effective 15-day deadline and Store expiration.
The Credential Locker marker is replaced or revoked by later Store checks. It
may remain in Windows Credential Locker after LocalState removal until Windows,
the user, or a later app run removes or replaces it; it cannot grant access
without a matching protected cache and is not usage-history data.
The Settings action for deleting usage data removes measurement sessions,
cached app metadata, and diary posts/notes. It does not reset preferences,
delete diagnostic logs, delete exported CSV/JSON files, delete the Store
license-resilience cache/security marker, or delete a pre-rename WinTrack source
database, migration marker, or migration backup.

Each diagnostic log rolls after approximately 512 KiB and keeps one previous
generation, for approximately 1 MiB per log name. Exported files remain until
the user deletes them from the location where they were saved. A legacy
WinTrack source database and migration backup are not subject to the 7/30/90-day
cleanup; users who no longer need them must delete those files manually.

### 5. Data sharing and user controls
Retime does not sell, rent, upload, or disclose app data. The current
version has no server component, account system, cloud sync, telemetry,
analytics, advertising, or remote crash reporting. Windows toast notifications
are generated locally and stay on the device.

License checks, trial status, localized pricing, and user-initiated purchases
are handled by Microsoft Store services. Those Store operations do not include
the usage history, diary, settings, exports, or diagnostic logs described in
this policy. Microsoft handles any Store account and payment information under
the [Microsoft Privacy Statement](https://privacy.microsoft.com/privacystatement).
The supported binary distribution, purchase, and update channel is Microsoft
Store only.

Users can change measurement retention, edit or delete diary posts, delete
usage history and diary data from Settings, and choose whether to create an
export. The app never uploads an export. The Live HUD starts off and is shown
only after the user enables it in Settings while tracking authorization remains
valid. Pausing tracking also stops the HUD from refreshing or displaying data.
If the Store license cannot be verified and no bounded protected fallback is
valid, or after the trial ends, tracking, Focus Timer, and Live HUD stop while
saved history remains available for in-app viewing and CSV/JSON export.

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
Retime has no account or online service and does not receive children's
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
Retime のインストール済み計測、レポート、エクスポート、HUD 機能は
ユーザーの Windows PC 内だけで動作し、アプリのデータを外部サービスへ送信しません。
アプリは、ライセンスと無料体験の有効期限の確認、Microsoft Store での表示価格の取得、
およびユーザーが選択した購入手続きに限り Microsoft Store サービスを利用します。
利用履歴、日記、設定、エクスポート、診断ログを Microsoft Store へ送信せず、
支払いカード情報を受け取りません。それ以外の外部への移動は、ユーザーが Privacy
Policy リンクを選択して既定ブラウザーでこの公開ページを開く場合だけです。
テレメトリ、アナリティクス、広告、クラウド同期、
外部クラッシュレポート SDK は含みません。例外発生時には
小さな診断ログを PC 内へ保存しますが、自動送信はしません。
任意機能の Live HUD は初期設定がオフで、有効な計測認可がある場合に限り、
端末内で処理された計測データだけを利用します。

### 2. 収集するデータ
Retime は、ユーザー自身が PC の使用時間を把握することだけを目的に、以下を
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

アプリは利用可否と Store 管理の購入情報を表示するためだけに、現在のライセンス状態、
無料体験かどうかと有効期限、および地域に応じた表示価格を Microsoft Store から読みます。
これらはアプリ利用記録ではなく、支払い情報を含みません。Store アカウント、権利、
無料体験、支払い処理は Retime ではなく Microsoft Store が管理します。
15日間の無料体験について Store が返す有効期限に小さな時刻の丸め差がある場合は、有界な範囲で
受理します。ただし実効期限は初回の確認から最長15日で、再確認や再起動で後ろへ動きません。
Store の確認に成功した後、Package Identity、Full／Trial の種別、確認日時、最終確認日時、
無料体験の有効期限だけを保護されたローカルキャッシュへ保存する場合があります。Store API を
一時的に利用できない場合、Full の fallback は最長30日間、Trial は初回の実効15日期限までで、
Store の無料体験期限を延長することはありません。
これとは別に、古いキャッシュだけを再利用できないよう、現在のユーザーの Windows 資格情報
マネージャーへ Package Identity、ランダムなキャッシュ世代、確認・観測日時、Trial の場合は
初回の実効期限、および失効フラグだけを保存します。パスワード、Store アカウント識別子、
支払い情報、利用履歴は含みません。
Windows の設定によりこのマーカーが Windows／Microsoft アカウント経由で同期される場合が
ありますが、Retime が送信するものではありません。
現在有効と Store が確認した無料体験は、キャッシュまたはマーカーを保存できないことだけでは
停止しません。ただし一致する保護情報がない間はオフライン fallback を利用できません。
無料体験中は、process-monotonic な経過時間を使って、保護された最終観測日時を定期的および
通常終了時に更新します。収集項目や Store アカウント情報が増えることはありません。

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
- ライセンス一時障害用の保護キャッシュ: Store パッケージの LocalState
  （Windows Data Protection により現在の Windows ユーザーへ暗号化・認証）
- ライセンスの再利用防止マーカー: 現在のユーザーの Windows 資格情報マネージャー
  （上記の限定されたセキュリティ情報のみ。利用履歴やアカウント・支払い情報は含みません）
- 旧 WinTrack 版からの移行時のみ: 元の
  `%LOCALAPPDATA%\WinTrack\wintrack.db` を残し、同じ場所に小さな移行済み
  マーカーを作成して、
  `%LOCALAPPDATA%\JustScreenTime\LegacyBackups\wintrack-v1.db` にバックアップ
  を作成します。Store 版では Windows のパッケージリダイレクトにより、
  バックアップがパッケージ内の `LocalCache` に置かれる場合があります
- CSV/JSON: ユーザーが保存時に明示的に選んだ場所

現在のDBとアプリが作成した旧版バックアップは、SQLite のジャーナルを含めて
ChaCha20-Poly1305 による認証付き暗号化で保存します。DBごとのランダムな鍵は、
隣接する `.key` ファイルに Windows Data Protection（DPAPI）で現在の Windows
ユーザーに結び付けて保護します。診断ログも DPAPI で保護します。旧版の平文DBは
コピーを暗号化して読み直しに成功した後に置き換えます。移行が中断した場合は
不完全なコピーで元DBを上書きせず、再試行します。旧診断ログはファイルが使用可能な
起動時または次回書き込み時に保護します。
バックアップではDBと対応する `.key` ファイルを一緒に保管し、元の Windows
アカウントで復元してください。鍵や Windows プロファイルを失うと、DBを復元できない
場合があります。Windows のアクセス制御も利用しますが、同じ Windows ユーザーで
動くソフトウェアや、そのアカウントを制御する管理者からのアクセスは防げません。
旧 WinTrack の元DBと、ユーザーが保存する CSV/JSON は従来の非暗号化形式を保ちます。
これらのファイルや以前の外部バックアップは別途保護してください。平文DBの移行で
使用する一時ファイルは、完了または再試行の成功後に削除します。

### 4. 保持期間
生の計測セッションの保持期間はユーザー指定（7／30／90 日、既定 90 日）です。
期限を過ぎたセッションは、計測が動作している間の次の定期クリーンアップで削除します。
計測停止中や Store の利用権限がない間はクリーンアップも停止し、計測再開後に
再開します。計測を止めただけで保存済み履歴が直ちに削除されることはありません。

日記と設定は、ユーザーが編集・削除するか、対応するアプリ内操作、DB 削除、
またはパッケージ版のアンインストールを行うまで保持します。
保護されたライセンスキャッシュは次回以降の Store 確認成功時に置き換えられ、
パッケージの LocalState 削除時に削除されます。Full の fallback は Store 確認から
最長30日間、Trial は初回の実効15日期限と Store 期限までです。資格情報マネージャーの
マーカーは、その後の Store 確認で
置換または失効されます。LocalState 削除後も Windows、ユーザー、または後のアプリ実行が
削除・置換するまで残る場合がありますが、一致する保護キャッシュなしでは権限を付与できず、
利用履歴ではありません。設定画面の
利用データ削除操作は、計測セッション、アプリ情報キャッシュ、日記投稿／メモを
削除します。設定、診断ログ、保存済み CSV/JSON、旧 WinTrack の元DB、
移行済みマーカー、移行バックアップ、Store ライセンス一時障害用キャッシュと
再利用防止マーカーは削除しません。

診断ログは各ファイルがおよそ 512 KiB でローテーションし、1 世代前まで
（ログ名ごとにおよそ 1 MiB）保持します。エクスポートファイルは、ユーザーが
保存先から削除するまで残ります。旧 WinTrack の元DBと移行バックアップは
7／30／90 日の自動削除対象ではなく、不要になった場合はユーザーが手動で
削除する必要があります。

### 5. データ共有とユーザー操作
Retime はアプリのデータを販売、貸与、アップロード、第三者提供しません。
現行版にはサーバー、アカウント、クラウド同期、テレメトリ、アナリティクス、
広告、外部クラッシュレポート機能がありません。Windows 通知は端末内で生成します。

ライセンス確認、無料体験の状態、地域に応じた表示価格、およびユーザーが選択した購入は
Microsoft Store サービスが処理します。これらの Store 操作に、このポリシーで説明する
利用履歴、日記、設定、エクスポート、診断ログは含まれません。Store アカウントと
支払い情報は [Microsoft プライバシー ステートメント](https://privacy.microsoft.com/privacystatement)
に基づいて Microsoft が取り扱います。正式なアプリ本体の配布、購入、更新経路は
Microsoft Store だけです。

保持期間の変更、日記の編集／削除、設定からの利用履歴・日記データ削除、
エクスポートするかどうかと保存先の選択ができます。エクスポートをアプリが
アップロードすることはありません。Live HUD は初期設定がオフで、計測認可が有効な
間にユーザーが設定画面で有効化した場合だけ表示されます。計測を停止すると、HUD の
データ更新と表示も停止します。
Store ライセンスを確認できず、有効期限内の保護キャッシュもない場合、または無料体験の
終了後は、計測、集中タイマー、Live HUD を停止します。保存済み履歴はアプリ内で閲覧でき、
CSV／JSON へ書き出せます。

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
Retime にはアカウントやオンラインサービスがなく、児童の個人情報を
運営者が受け取ることはありません。児童が利用した場合は、上記と同じローカル記録が
児童本人または共有の Windows プロファイル内に作成される場合があります。

### 7. 変更
このポリシーを変更する場合は、新しいアプリバージョンと共に Microsoft Store で
使用する公開 Privacy Policy URL に掲載し、リリースノートに要約を記載します。

### 8. お問い合わせ
**taiman.jp@gmail.com**
