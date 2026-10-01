---
layout: page
title: Privacy Policy
permalink: /just-screentime/privacy/
---

# Retime Privacy Policy / プライバシーポリシー

Retime is the new name of Just ScreenTime. / Retime は Just ScreenTime の新しい名称です。

**Last updated / 最終更新:** 2026-10-01
**Applies to / 対象:** Retime 1.2.14 (after installing this version / この版のインストール後)

**Version notice / バージョンについて:** Version 1.2.14 makes the base app free and introduces separately consented, optional page-usage sharing. Updating this policy does not enable sharing in older installations. Versions 1.2.12–1.2.13 use Store full/trial licensing and do not send page-usage reports. Database and diagnostic-log encryption applies from version 1.2.9 after successful local migration. / 1.2.14では本体を無料化し、別途同意した場合だけ画面の利用状況を共有する機能を追加します。このページの更新だけで旧版から送信が始まることはありません。1.2.12〜1.2.13はStoreのFull／Trialライセンスを使い、画面利用状況を送信しません。DB・診断ログの暗号化は1.2.9以降でローカル移行に成功した後に適用されます。

## English

### 1. Summary

**Your diary text is never sent to the server.** This remains true when optional page-usage sharing is enabled.

Screen-time histories, diary text, settings and diagnostic logs stay on your Windows PC. The free base app works without a Store purchase or trial deadline. Tracking still requires your explicit consent; Live HUD is optional and initially off. Home and Diary use the same saved diary entries. The holiday calendar uses bundled data and does not access a calendar account or download holiday data.

Optional **page-usage sharing** is separate from screen-time tracking. It starts off for both new and existing users. After the explanation, you may choose to help improve Retime and inform future advertising placement. Only then does the app automatically send coarse weekly page totals to the developer's service hosted on Cloudflare Workers and D1. Declining leaves all free features available. An old local-only statistics choice does not authorize sharing.

There is currently no advertising SDK, ad display, cloud sync, account system or remote crash-reporting SDK. Page sharing does not authorize future advertising. The developer does not sell diary contents or app histories, and those records are not included in page reports.

### 2. Local data

After tracking consent and while tracking is enabled, the app records:

- Foreground executable paths and process names, and per-app start/end times and durations, divided into Active, Foreground Idle and Background sessions.
- Optional diary posts, post times and the active app name/path attached locally as context. If separately enabled, a weekly digest creates a local diary summary after 18:00 on Sunday.
- Settings including retention, idle threshold, theme, shell exclusion, goals, holiday countries, tracking authorization, optional page-sharing choice and Live HUD preferences.
- App display names and icons derived from local executable files and cached for the UI and HUD.
- Small local error logs when an exception occurs. These may include component names, exception messages, stack traces, timestamps and local file paths. They are never automatically uploaded.
- CSV/JSON files that you explicitly save locally.

The app does not record window titles, document contents, URLs, the characters you type, clipboard contents, screenshots, microphone, camera or network traffic. Page timing observes activation and input events only in Retime's own window; it does not retain key values or pointer coordinates.

The HUD reads tracker-written local state and does not independently inspect other processes. If tracking authorization is absent, declined, disabled or invalid, the HUD cannot be enabled and is stopped fail-closed.

### 3. Optional automatic sharing

If you agree, the app counts visits and foreground time only for Home, Day detail, Diary and Settings. Returning the window to the foreground counts as a visit. Background/minimized time, sampling gaps over 30 seconds and time after five minutes without interaction are excluded. These are estimates of page use, not proof that a person looked at an advertisement. Weeks start Monday in UTC.

A report contains only:

- A UTC week and one of the four fixed page names.
- Cumulative visit counts and foreground duration, rounded down to whole seconds.
- The app version at the first report for that week. If updated within a week, that week's aggregate can cover more than one version.
- A random weekly report ID and revision number to replace retried reports without double-counting. This is not a persistent installation/device or account ID and is not shared across weeks. It is still a temporary report identifier, not a promise of absolute anonymity.

No diary text, screen-time sessions, other app names/paths, exact navigation timestamps, account details, advertising ID or diagnostic log is included. No third-party analytics SDK runs in the app.

Reports go by HTTPS to the developer's Cloudflare Workers service. The destination is fixed in the app build. Requests carry no app-supplied cookies or account credentials and do not follow redirects. At most one report is attempted per 24 hours while the app runs; failures can be retried on a later day. Old pending weeks may delay the newest report. Network failure never blocks free features.

Cloudflare necessarily receives the source IP address and connection metadata to handle the request. The developer's statistics database stores the report fields above and the first/latest receipt times used to manage retention. These are receipt times, not page-navigation events. It does not store IP addresses or request headers, and the Worker does not log request bodies. Cloudflare's own infrastructure/security processing and backups are governed by its [Privacy Policy](https://www.cloudflare.com/privacypolicy/) and service terms. Processing may occur outside your country. The reports are accessible to the developer through the authenticated Cloudflare account; no public report-reading API is provided.

Turn sharing off at any time in Settings. This stops new uploads, cancels in-flight work where possible and deletes local totals and pending report identities. A request already received by the server cannot be recalled by cancelling the connection; already received aggregates remain subject to the server retention below. Re-enabling starts a new consent period and does not send earlier local-only totals. You can review the local totals and explicitly save a JSON copy. That saved copy is not uploaded by the save action itself.

Developer builds without a configured destination offer local-only page statistics instead, with their own off-by-default choice and no network sender enabled.

### 4. Storage, protection and retention

- The Store database is under `%LOCALAPPDATA%\Packages\<PackageFamilyName>\LocalCache\Local\JustScreenTime\justscreentime.db`. Private unpackaged development builds normally use `%LOCALAPPDATA%\JustScreenTime\justscreentime.db`.
- The database and app-owned legacy backup use SQLite3MC ChaCha20-Poly1305 authenticated encryption, including SQLite journals. A random database key is in the adjacent `.key` file, protected with Windows DPAPI for the current Windows user. Error logs are DPAPI-protected too.
- Plaintext upgrades verify an encrypted copy before atomic replacement. Interrupted migration does not replace the original with an incomplete copy. Missing/corrupt keys never cause an existing database to be silently recreated or erased.
- Keep a database and its `.key` file together under the original Windows account. Losing the key or profile may make the data unrecoverable. Encryption does not protect against software or an administrator controlling the same Windows account.
- Raw screen-time sessions are retained for the selected 7, 30 or 90 days (default 90), and removed by cleanup while tracking runs. Pausing tracking stops cleanup but does not immediately delete saved history. Diary entries and preferences remain until you delete them.
- Local page totals cover the current UTC week and seven previous weeks. Pruning runs when the app initializes, collects or prepares a report. Data cannot be removed by the app while it is not running. Turning the option off removes all local page totals immediately.
- Server reports older than 90 days from first receipt are deleted on the next daily cleanup or incoming report. Provider-managed recovery backups may retain deleted data for their configured recovery period; this is separate from the active statistics database.
- Each error log rolls at about 512 KiB and retains one earlier generation (about 1 MiB per log name).

The Settings action to delete usage data removes sessions, app metadata, diary posts/notes and local page totals; it also turns optional page sharing off. Other preferences remain. It does not erase diagnostic logs, previously saved exports, already received server reports, or old migration/license files.

A pre-rename WinTrack source DB is preserved at `%LOCALAPPDATA%\WinTrack\wintrack.db`, with a migration marker and an app-owned backup at `%LOCALAPPDATA%\JustScreenTime\LegacyBackups\wintrack-v1.db` (subject to package path redirection). The original source and explicit CSV/JSON exports remain unencrypted and are outside normal retention. Older protected Store license caches in LocalState and anti-replay markers in Windows Credential Locker are left unused by 1.2.14. They contain package/license/security fields rather than usage histories; Windows may synchronize Credential Locker according to the account's settings. Users can remove unneeded old files separately.

Uninstalling the Store package normally removes package-local data and logs. Unpackaged data, original legacy files, exports and provider-held reports are not removed just by uninstalling. Server reports expire as described above.

### 5. External services and choices

Microsoft Store handles app distribution and updates under the [Microsoft Privacy Statement](https://privacy.microsoft.com/privacystatement). Version 1.2.14 does not call Store purchase/trial APIs to unlock the free base features. Earlier versions may have protected, time-limited license caches and stop recording after trial expiry; installing 1.2.14 removes that base-feature restriction without deleting your history.

Selecting the Privacy Policy link opens this site in your default browser. It is hosted on GitHub Pages; GitHub may retain visitor IP addresses for security. The developer adds no website analytics, ads, forms, cookies or tracking scripts to this policy page. Website access is separate from app sharing.

Retime has no user accounts and the developer does not intentionally request children's personal data. Users who do not wish to share can decline or switch the option off. Advertisements and paid ad removal are not part of 1.2.14; any later change will have its own explanation and applicable choices.

### 6. Changes and contact

Material data-practice changes are described in the app and this policy before enabling new collection or sharing. Updating terms or continuing to use the app alone does not enable optional page sharing. Questions or requests: **taiman.jp@gmail.com**.

## 日本語

### 1. 概要

**日記本文はサーバーに送信しません。** 画面利用状況の共有をオンにしても、日記本文は送信対象になりません。

スクリーンタイムの利用履歴、日記本文、設定、診断ログはWindows PC内に保存します。基本機能は無料で、Storeでの購入や試用期限に依存しません。計測には引き続き明示的な同意が必要です。Live HUDは任意で、初期値はオフです。ホームと日記ページは同じ保存済み日記を使います。祝日カレンダーは同梱データを使い、カレンダーアカウントや外部ダウンロードを利用しません。

任意の**画面利用状況の共有**は、スクリーンタイム計測とは別の設定です。新規・既存ユーザーとも初期値はオフです。説明を読んで改善と今後の広告配置の検討への協力を選んだ場合だけ、画面別の週次集計を、開発者がCloudflare WorkersとD1で運営する受信サービスへ自動送信します。拒否しても全無料機能を使えます。以前の「端末内だけの集計」への同意を送信の許可には使いません。

現行版には広告SDK・広告表示・クラウド同期・ユーザーアカウント・外部クラッシュ送信SDKはありません。今回の共有への同意は、将来の広告表示への同意を兼ねません。日記本文や他アプリの利用履歴を開発者が販売することはなく、画面利用状況レポートにも含めません。

### 2. PC内に保存するデータ

計測への同意後、計測が有効な間に以下を保存します。

- 前面アプリの実行ファイルパス・プロセス名と、アプリ別の開始・終了時刻、継続時間。Active・Fg-Idle・Backgroundに分類します。
- 任意の日記本文、投稿日時、文脈として端末内だけで添付するアクティブなアプリ名とパス。別途有効化した週次ダイジェストは日曜18時以降にローカル日記要約を作ります。
- 保持日数、アイドル閾値、テーマ、シェル除外、目標、祝日の国、計測認可、任意共有の選択、HUDなどの設定。
- ローカルの実行ファイルから得た表示名・アイコンのキャッシュ。
- 例外発生時の小さな診断ログ。コンポーネント、例外内容、スタックトレース、日時、ローカルパスを含む場合がありますが、自動送信しません。
- ユーザーが明示的に保存したCSV／JSONファイル。

ウィンドウタイトル、文書内容、URL、入力した文字、クリップボード、スクリーンショット、マイク、カメラ、ネットワーク通信内容は記録しません。画面滞在時間の計測ではRetime自身のウィンドウの表示・入力イベントだけを参照し、キーの値やポインター座標を保存しません。

HUDは計測コンポーネントがローカルDBへ記録した情報を読み、独自に他のプロセスを調査しません。計測認可が未取得・拒否・無効・不正の場合、HUDは有効化できず停止します。

### 3. 任意の自動送信

同意した場合だけ、ホーム・日別詳細・日記・設定の4画面について、表示回数と前面表示時間を数えます。前面へ戻した場合も1回と数えます。背面・最小化中、30秒を超えたサンプル間隔、操作から5分を超えた放置時間は除外します。これは画面利用の目安であり、広告を実際に見た時間の証明ではありません。週の区切りはUTCの月曜日です。

送信する項目は次のものに限ります。

- UTCの週と、4種類に限定した画面名。
- 累積表示回数、秒未満を切り捨てた前面表示時間。
- その週に初めてレポートを送信した時点のアプリ版。同じ週に更新した場合、集計には複数の版の利用が混在する場合があります。
- 再送による二重計上を防ぐ週単位のランダムなレポートIDと改訂番号。固定の端末・インストール・アカウントIDではなく、別の週には引き継ぎません。ただし一時的なレポート識別子なので、完全な匿名性を保証するものではありません。

日記本文、スクリーンタイムのセッション、他アプリの名前やパス、正確な画面遷移時刻、アカウント情報、広告ID、診断ログは送りません。第三者の解析SDKも使用しません。

送信先はアプリのビルド時に固定した、開発者のCloudflare Workersサービスです。HTTPSを使い、Cookieやアカウント認証情報を付けず、リダイレクトを追跡しません。アプリ起動中に24時間あたり最大1件を試行し、通信に失敗した場合は後日再試行することがあります。未送信の古い週がある場合、最新週の送信が遅れることがあります。通信の失敗で無料機能が使えなくなることはありません。

通信のため、接続元IPアドレスや接続メタデータはCloudflareへ届きます。開発者の集計DBには上記レポート項目と、保持期間の管理に使う初回・最終受信時刻を保存します。これは受信時刻であり、画面遷移の時刻ではありません。IPアドレスやリクエストヘッダーは保存せず、Workerもリクエスト本文をログへ出力しません。Cloudflare自身のインフラ運用・セキュリティ処理・バックアップには、同社の[プライバシーポリシー](https://www.cloudflare.com/privacypolicy/)とサービス条件が適用されます。処理が居住国の外で行われる場合があります。集計結果は開発者の認証済みCloudflareアカウントから確認し、公開の閲覧APIは設けません。

設定でいつでも共有をオフにできます。新しい送信を停止し、可能な範囲で送信中の処理を中止して、PC内の集計と未送信レポートIDを削除します。既にサーバーへ到着したリクエストは接続中止では取り消せず、送信済み集計は下記のサーバー保持期間に従います。再度オンにした場合は新しい同意期間として開始し、以前の端末内専用の集計を送りません。PC内の集計を確認してJSONを明示的に保存できますが、その保存操作自体がファイルをアップロードすることはありません。

送信先を設定していない開発用ビルドでは、別の初期値オフの設定で端末内集計だけを提供し、送信機能は有効にしません。

### 4. 保存場所・保護・保持期間

- Store版DBは `%LOCALAPPDATA%\Packages\<PackageFamilyName>\LocalCache\Local\JustScreenTime\justscreentime.db`、非パッケージの開発版は通常 `%LOCALAPPDATA%\JustScreenTime\justscreentime.db` に保存します。
- DBとアプリ管理の旧版バックアップは、SQLiteのジャーナルも含め、SQLite3MCのChaCha20-Poly1305認証付き暗号化を使います。ランダムなDB鍵は隣接する `.key` ファイルに、現在のWindowsユーザーのDPAPIで保護して保存します。診断ログもDPAPIで保護します。
- 平文DBの更新では暗号化コピーを検証してから置き換えます。中断時に不完全なコピーを元DBへ上書きせず、鍵の欠損・破損を理由に既存DBを黙って作り直したり消したりしません。
- バックアップはDBと `.key` の両方を元のWindowsアカウントで保存してください。鍵やプロファイルを失うと復元できない場合があります。同じアカウントを制御するソフトウェアや管理者からのアクセスまで防ぐものではありません。
- 生の利用セッションは選択した7／30／90日（既定90日）を保持し、計測中のクリーンアップで削除します。計測停止中はクリーンアップも停止し、停止だけで履歴が消えることはありません。日記や通常の設定はユーザーが削除するまで保持します。
- PC内の画面別集計はUTCの今週を含め8週間分です。起動・集計・レポート準備時に期限を過ぎたものを削除します。アプリが動いていない間に削除処理は実行できません。設定をオフにしたときは端末内の全画面集計を削除します。
- サーバーの集計は最初の受信から90日を過ぎた後、次の日次削除処理またはレポート受信時に削除します。Cloudflare管理の復旧用バックアップには、設定された復旧期間中データが残る場合があります。これは稼働中の集計DBとは別です。
- 診断ログは約512 KiBでローテーションし、1世代前まで、ログ名ごとに合計約1 MiBを保持します。

設定の「利用データ削除」はセッション、アプリ情報、日記投稿・メモ、PC内の画面集計を削除し、任意の共有をオフにします。それ以外の設定は保持します。診断ログ、保存済みエクスポート、送信済みのサーバー集計、旧版移行・ライセンス関連ファイルは消しません。

旧WinTrackの元DB `%LOCALAPPDATA%\WinTrack\wintrack.db` は移行マーカーとともに残し、アプリ管理のバックアップを `%LOCALAPPDATA%\JustScreenTime\LegacyBackups\wintrack-v1.db` に保存します（Store版ではパッケージ内へリダイレクトされる場合があります）。元DBと明示保存したCSV／JSONは平文のままで、通常の保持期間の対象外です。旧版がLocalStateへ保存した保護ライセンスキャッシュとWindows資格情報マネージャーの再利用防止マーカーは1.2.14では参照せず、そのまま残します。これらは利用履歴ではなくパッケージ・ライセンス・安全確認用の情報で、資格情報マネージャーの同期はWindowsの設定に従います。不要な旧ファイルは個別に削除できます。

Store版のアンインストールでは通常、パッケージ内のDB・ログが削除されます。非パッケージ版のデータ、旧版の元ファイル、エクスポート、サーバーにある集計はアンインストールだけでは消えません。サーバー集計は上記の期限で削除されます。

### 5. 外部サービスと選択

アプリの配布・更新はMicrosoft Storeが[Microsoftのプライバシーステートメント](https://privacy.microsoft.com/privacystatement)に基づいて行います。1.2.14は基本機能の解除のためにStoreの購入・試用APIを呼びません。旧版は保護された期限付きのライセンスキャッシュを使い、試用後に計測を止めることがあります。1.2.14への更新は保存済み履歴を消さずに基本機能の制限を解除します。

Privacy Policyリンクを選ぶと、この公開ページを既定のブラウザーで開きます。GitHub Pagesでホストしており、GitHubが安全上の目的で閲覧者のIPアドレスを保持する場合があります。開発者はこのポリシーサイトへ独自の解析・広告・フォーム・Cookie・追跡スクリプトを追加しません。サイトの閲覧とアプリからの共有は別です。

Retimeにはユーザーアカウントがなく、開発者が児童の個人情報を意図して求めることはありません。共有を望まない場合は拒否または設定で停止できます。1.2.14には広告表示や広告削除課金は含まれず、将来導入する場合は別途説明と必要な選択肢を設けます。

### 6. 変更とお問い合わせ

収集・共有の重要な変更は、有効化の前にアプリ内とこのポリシーで説明します。利用規約の更新や使い続けることだけで任意の共有を有効にはしません。ご質問・ご要望: **taiman.jp@gmail.com**。
