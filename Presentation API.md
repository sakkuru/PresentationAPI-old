# Presentation API

Editor's Draft 18 March 2015

## 概要

本仕様はWebコンテンツに外部プレゼンテーションタイプのディスプレイにアクセスし、Webコンテンツを表示することを可能にするAPIを定義する。

## 本ドキュメントの状態

## 1. 導入

本セクションは非規範的である。

本仕様はプロジェクタや接続されたTVなどのセカンダリディスプレイに、Webを利用できるようにすることを目的としている。有線（HDMI, DVI等）および無線技術(MiraCast, Chromecast, DLNA, AirPlay等)で接続されているディスプレイを考慮に入れる。

<!--
Devices with limited screen size lack the ability to show content to a larger audience, for example a group of colleagues in a conference room, or friends and family at home. Showing content on an external large display helps to improve the perceived quality and impact of the presented content.
-->

限られたスクリーンサイズのデバイスは、多くの聴衆に向けてのコンテンツ表示能力に欠ける。例えばカンファレンスルームでの同僚グループや、宅内での友人や家族など。
外部の大きなディスプレイへのコンテンツ表示は、表示コンテンツの認識の質やインパクトを向上させる。

<!--
At its core, this specification enables an exchange of messages between a requesting page and a presentation page shown in the secondary display. How those messages are transmitted is left to the UA in order to allow for use of display devices that can be attached in a wide variety of ways. 
For example, when a display device is attached using HDMI or MiraCast, the UA on the requesting device can render the requested presentation page in that same UA, 
but instead of displaying in a window on that same device, it can use whatever means the operating system provides for using those external displays. In that case, both the requesting page and the presentation page run on the requesting device and the operating system is used to route the presentation display output to the other display device. The second display device doesn't need to know anything about this spec or that the content involves HTML5.
-->

中核として、本仕様はリクエストを送るページとセカンダリディスプレイに表示されるページとの間でメッセージ交換を可能にする。
さまざまな方法で接続されているディスプレイデバイスを使用できるようにするため、どのようにメッセージが送信されるかはUAに委ねられる。
例えば、ディスプレイデバイスがHDMIやMiraCastで接続されている場合は、リクエストを送信するデバイス上のUAは、同じUA上でリクエストされたページを表示できる。
また同じデバイス上のウィンドウで表示する代わりに、OSが提供する外部ディスプレイを使うためのどんな方法でも使うことができる。
この場合、リクエストを送るページも表示されるページもリクエストを送るデバイスで動いていて、OSはディスプレイ出力を他のディスプレイデバイスに送るために使われる。
セカンドディスプレイデバイスは、本仕様やコンテンツがHTML5を含むことについて知る必要は無い。

<!-- Alternately, some types of external displays may be able to render HTML5 themselves and may have defined their own way to send messages to that content. In that case, the UA on the requesting device would not need to render the presentation page itself. Instead, the UA could act as a proxy translating the request to show a page and the messages into the form understood by the display device.
 -->

代わりに、いくつかのタイプの外部ディスプレイは、それ自身でHTML5を表示できるかもしれない。
さらに、メッセージをそのコンテンツに対して送る手段を持っているかもしれない。
この場合、リクエストを送るデバイス上のUAは、表示したいページをそれ自身で表示する必要がない。
その代わりUAは、ページを表示するためのリクエクストとメッセージを、ディスプレイデバイスが理解できる形へ変換するプロキシとして動作するだろう。

<!-- 
This way of attaching to displays could be enhanced in the future through definition of a standard protocol for delivering these types of messages that display devices could choose to implement.
 -->

このディスプレイへ接続する方法は、ディスプレイデバイスが実装を選択するメッセージを送信するための標準プロトコルの定義により、将来的に改善・強化されるだろう。

本APIは上記の方法のいずれかでディスプレイデバイスと接続されているUAで使用されることを意図している。



## 2 ユースケースと要件

### 2.1 ユースケース

#### プレゼンテーション
<!-- 
A user is preparing a set of slides for a talk. Using a web based service, she is editing her slides and speaker notes on the primary screen, while the secondary larger screen shows a preview of the current slide. When the slides are done, her mobile phone allows her to access them from an online service while on the go. Coming to the conference, using wireless display technology, she would like to present her slides on the stage screen from her mobile phone. The phone's touch screen helps her to navigate slides and presents a slide preview, while the projector shows her slides to the audience.
 -->


あるユーザが講演用のスライドを用意する。
彼女は大きいセカンドスクリーンに現在のスライドのプレビューを表示させながら、
Webベースのサービスを使ってスライドとスピーカーメモを編集する。
スライドが完成したとき、彼女の携帯電話は外出中オンラインサービスからそれらへアクセスを可能にする。
カンファレンスに来て、無線ディスプレイ技術を使用して、彼女は携帯電話からスライドをステージスクリーンに表示したいと思う。
プロジェクタがスライドを聴衆に見せる間、電話のタッチスクリーンはスライドをナビゲートし、スライドプレビューを表示することを助ける。

#### ビデオ・画像共有

<!-- 
Using an online video or image sharing service, a user would like to show memorable moments to her friends. Using a device with a small screen, it is impossible to show the content to a large group of people. Connecting an external TV screen or projector to her device - with a cable or wirelessly - the online sharing service now makes use of the connected display, allowing a wider audience to enjoy the content. The web page shows UI elements that allow the user to trigger displaying content on the secondary display (e.g a "send to second screen" ) only if there is at least one secondary screen available.
 -->

オンラインビデオ・画像共有サービスを使って、あるユーザは記憶したい瞬間を友人たちに見せたいと思う。
小さなスクリーンのデバイスでは、大勢の人間にコンテンツを見せることは不可能である。
デバイスを外部TVスクリーンやプロジェクタへ接続(有線もしくは無線で)し、オンライン共有サービスは接続されたディスプレイを使用し、多くの聴衆がコンテンツを楽しめるようにする。
少なくとも一つのセカンドスクリーンが使用可能なときだけ、Webページはユーザがコンテンツをセカンドディスプレイに表示開始できるUI要素を表示する。

#### マルチプレイヤーゲーム
<!-- 
Splitting the gaming experience into a near screen controller and a large screen visual experience, new gaming experiences can be created. Accessing the local display on the small screen device and an external larger display allows for richer web-based gaming experiences.
 -->

ゲームエクスペリエンスを近くのスクリーンコントローラと大きなスクリーンビジュアル経験に分けることで、
新しいゲーム経験は作られるだろう。
小さなスクリーンデバイス上のローカルディスプレイと、大きな外部ディスプレイににアクセスすることは、よりリッチなWebベースのゲームエクスペリエンスをもたらす。

#### 複数スクリーンへのメディア表示

<!-- 
Alice enters a video sharing site using a browser on her tablet. Next, Alice picks her favorite video from the site, and the video starts to play on her tablet. While the video is playing Alice clicks a button "Share on different screen". The browser provides a user interface that lists all the screens Alice has around her home, asking her to select one. The screens are identified by names that are familiar to Alice. Alice picks one screen from the list, "Alice's big TV", and the video playback continues seamlessly on the selected screen. Next she decides to switch the playback to a different screen. She clicks the same button "Share on different screen" provided by the site, and the browser presents the user interface that lists all the screens. Alice picks another screen from the list, "Alice's kitchen TV", and the playback resumes on that screen. Video site also provides a feature to see the action (Alice is watching a soccer game) from different angle. Alice clicks a button "Select screen for additional angle", and the browser asks Alice similarly to select the screen to be used for playback. Alice picks "Alice's Projector" and the soccer game is shown on the projector from a different angle, in parallel to the content being played back on "Alice's kitchen TV".
 -->

アリスはタブレットのブラウザからビデオ共有サイトにアクセスする。
次にアリスは彼女のお気に入りのビデオをサイトから選択すると、彼女のタブレットでビデオが流れる。
ビデオが流れている間に、アリスが「別のスクリーンで共有」ボタンをクリックする。
ブラウザは、アリスの家にあるすべてのスクリーンをリストアップしたユーザ・インタフェースを表示し、彼女にどれか一つを選択するよう問う。
スクリーンはアリスにとって馴染みのある名前で識別される。
アリスはリストから一つのスクリーン、『アリスの大きいテレビ』を選ぶと、ビデオは選択されたスクリーンでシームレスに続けて再生される。
次に彼女は再生スクリーンを切り替えることを決める。
彼女がサイトにある同じボタン「別のスクリーンで共有」でクリックすると、ブラウザはすべてのスクリーンのリストしたユーザインタフェースを表示する。
アリスはリストから別のスクリーン「アリスのキッチンのTV」を選択すると、そのスクリーンで再生が始まる。
ビデオサイトはまた、(アリスがサッカーの試合を見ているとき)異なるアングルからアクションを見る機能を提供する。
アリスが『他のアングル用のスクリーンを選択』ボタンをクリックすると、ブラウザは同様にアリスに再生用のスクリーンを選択するよう尋ねる。
アリスは「アリスのプロジェクタ」を選択すると、「アリスのキッチンのTV」で再生されているコンテンツと並行して、別のアングルのサッカーの試合がそのプロジェクタから表示される。


### 2.2 要件

#### 2.2.1 機能要件

##### 検出・可用性
<!-- 
The UA must provide a way to find out whether at least one secondary screen is available.
 -->

UAは、少なくとも一つのセカンダリスクリーンが使用できるかどうかを発見する方法を提供しなければならない。

##### プレゼンテーションの開始

<!-- 
The UA must provide a means of start sending content to the secondary screen.
 -->

UAはセカンドディスプレイへコンテンツ送信を開始する方法を提供しなければならない。

##### プレゼンテーションの再開

<!-- 
The UA must be able to resume an existing session with content being displayed on the secondary screen.
 -->

UAはセカンドディスプレイに表示されているコンテンツの既存のセッションの再開が出来なければならない

##### 通信

<!-- 
The UA must enable exchanging data between the primary and the secondary screen in order to have a control channel between the primary and secondary page. The UA must not make assumptions about the the execution locality of the user agent of the remote page it communicates with (i.e. the secondary page might run on a remote user agent and thus the link between the two pages' UA must be loosely coupled).
 -->

UAはプライマリとセカンダリページの間でコントロールチャネルを持つために、プライマリとセカンダリスクリーンの間でデータ交換を可能にしなければならない。
UAは自身が通信しているリモートページのUAの実行局地性について憶測を立ててはならない（例：セカンダリページはリモートUA上で動いているだろう、だから2つのページのUA間は疎結合に違いない）

##### シグナリング切断

<!-- 
The UA must signal disconnection from the presentation page to the primary page and vice versa.
UAは表示ページから開始ページへ(その逆も)、切断信号を送れなければならない。
 -->

#### 2.2.2 非機能要件


##### パワーセーブフレンドリー
<!-- 
All API design decisions must be analyzed from a power efficiency point of view. Especially when using wireless display technologies or querying availability over a radio channel, care needs to be taken to design the API in a way that does not pose obstacles to using radio resources in an efficient way. For example, powering up the wireless display detection only when needed.
 -->

すべてのAPI設計決定はパワー効率性の観点から分析されなければならない。
特に無線表示技術を使用するときや、無線チャネル上で可用性の問い合わせをするとき、
APIは効率的な方法での無線リソースの使用に障害とならない方法で設計されるよう注意する必要がある。


## 3 適合性

省略

## 4 用語

省略

## 5 例

<!-- 
This section shows example codes that highlight the usage of main features of the Presentation API. In these examples, controller.html represents the controlling page that runs in the opener user agent and presentation.html represents the presenting page that runs in the presenting user agent. Both pages are served from the domain http://example.org (http://example.org/controller.html and http://example.org/presentation.html). Please refer to the comments in the code examples for further details.
 -->

本セクションでは、Presentation APIの主な特徴の使用を強調したサンプルコードを紹介する。
これらの例では、controller.htmlはオープナーUAで動くコントロールページを表し、presentation.htmlはプレゼン用UAで動作するプレゼンテーションページを表す。
どちらのページもhttp://example.orgドメイン(http://example.org/controller.htmlとhttp://example.org/presentation.html)にホストされる。
詳細はコード中のコメントを参照。

### 5.1 プレゼンテーションディスプレイの可用性監視


```
<!-- controller.html -->
<script>
  // it is also possible to use relative presentation URL e.g. "presentation.html"
  var presUrl = "http://example.com/presentation.html";
  // create random presId 
  var presId = Math.random().toFixed(6).substr(2);
  // Start new session. presId is optional.
  // the new started session will be passed to setSession on success or null on error
  navigator.presentation.startSession(presUrl, presId).then(setSession).catch(function(e){
    // user cancels the selection dialog or an error is occurred
    setSession(null);
  });
</script>
```

### 5.2 新規プレゼンテーションセッションの開始

```
<!-- controller.html -->
<script>
  // read presId from localStorage if exists 
  var presId = localStorage && localStorage["presId"] || null;
  // presId is mandatory for joinSession.
  // The joined session will be passed to setSession on success or null on error
  presId && navigator.presentation.joinSession(presUrl, presId).then(setSession).catch(function(e){
    // no session found for presUrl and presId or an error is occurred
    setSession(null);
  });
</script>
```

### 5.3 プレゼンテーションセッションへの参加

```
<!-- controller.html -->
<script>
  // read presId from localStorage if exists 
  var presId = localStorage && localStorage["presId"] || null;
  // presId is mandatory for joinSession.
  // The joined session will be passed to setSession on success or null on error
  presId && navigator.presentation.joinSession(presUrl, presId).then(setSession).catch(function(e){
    // no session found for presUrl and presId or an error is occurred
    setSession(null);
  });
</script>
```

### 5.4 セッション情報の監視とデータ交換

```
<!-- controller.html -->
<script>
  var session;
  var setSession = function(theSession){
    // close old session if exists
    session && session.close();
    // remove old presId from localStorage if exists
    localStorage && delete localStorage["presId"];
    // set the new session
    session = theSession;
    if(session){
      // save presId in localStorage
      localStorage && localStorage["presId"] = session.id;
      // monitor session's state
      session.onstatechange = function(){
        if(this == session && this.state == "disconnected")
          setSession(null);
      };
      // register message handler
      session.onmessage = function(msg){
        console.log("receive message",msg);
      };
      // send message to presentation page
      session.postMessage("say hello");
    }
  };
</script>
<!-- presentation.html -->
<script>
  var session = navigator.presentation.session;
  session.onstatechange = function(){
    // session.state is either 'connected' or 'disconnected'
    console.log("session's state is now", session.state);
  };
  session.onmessage = function(msg){
    if(msg == "say hello")
      session.postMessage("hello");
  };
</script>
```

## 6 API

### 6.1 共通の単語
<!-- 
A presentation display refers to an external screen available to the user agent via an implementation specific connection technology.
 -->

presentation displayは、特定の接続技術の実装を通してUAが利用可能な外部ディスプレイを指す。

<!-- 
A presentation is an active connection between a user agent and a presentation display for displaying web content on the latter at the request of the former.
 -->

presentationは、UAとプレゼンテーションディスプレイの間の、前者のリクエストに応じて後者にWebコンテンツを表示するためのアクティブな接続である。

<!-- 
A presentation session is an object relating an opening browsing context to its presentation display and enabling two-way-messaging between them. Each such object has a presentation session state and a presentation session identifier to distinguish it from other presentation sessions.
 -->

presentation sessionはopening browsing contextとpresentation displayを関連付け、また2つの間の双方向メッセージングを可能にするオブジェクトである。
各オブジェクトはpresentation session stateと、他のpresentation sessionと区別するためのpresentation session identifierを持つ。

<!-- 
An opening browsing context is a browsing context that has initiated or resumed a presentation session by calling startSession() or joinSession().
 -->

オープニングブラウジングコンテキストは、startSession()かjoinSession()の呼び出しにより、プレゼンテーションセッションを開始もしくは再開したブラウジングコンテキストである。

<!-- 
The presenting browsing context is the browsing context responsible for rendering to a presentation display. A presenting browsing context can reside in the same user agent as the opening browsing context or a different one.
 -->

presenting browsing contextは、プレゼンテーションディスプレイに表示を行う責任があるbrowsing contextである。
presenting browsing contextは、opening browsing contextや、他のものと同じUA内に存在できる。


### 6.2 共通の定義

<!-- 
Let D be the set of presentations that are currently known to the user agent (regardles of their state). D is represented as a set of tuples (U, I, S) where U is the URL that is being presented; I is an alphanumeric identifier for the presentation; and S is the user agent's PresentationSession for the presentation. U and I together uniquely identify the PresentationSession of the corresponding presentation.
 -->

Dを、UAによって現在知られている一連のpresentationとする(状態にかかわらず)。
Dは一組のタプル(U,I,S)として表される。
Uは表示されているURL、Iはpresentation用の英数字の識別子、SはpresentationのためのUAのPresentationSessionである。
UとIは、対応するpresentationのPresentationSessionを一意的に識別する。

### 6.3 PresentationSessionインタフェース

各presentation sessionは、PresentationSessionオブジェクトによって表される。

```
enum PresentationSessionState { "connected", "disconnected" /*, "resumed" */ };
enum BinaryType { "blob", "arraybuffer" };

interface PresentationSession : EventTarget {
  readonly DOMString? id;
  readonly attribute PresentationSessionState state;
  void close();
  attribute EventHandler onstatechange;

  // Communication
  attribute BinaryType binaryType;
  EventHandler onmessage;
  void postMessage (DOMString message);
  void postMessage (Blob data);
  void postMessage (ArrayBuffer data);
  void postMessage (ArrayBufferView data);
};
```

<!-- 
The id attribute holds the alphanumeric presentation session identifier.
 -->

id属性は、英数字のpresentation session identifierを保持する。

<!-- 
The state attribute represents the presentation session's current state. It can take one of the values of PresentationSessionState depending on connection state.
 -->

state属性はpresentation sessionの現在の状態を表す。
接続状態に応じて、PresentationSessionStateの値のいずれかを取る。

<!-- 
When the postMessage() method is called on a PresentationSession object with a message, the user agent must run the algorithm to post a message through a PresentationSession.
 -->

messageとともにPresentationSessionオブジェクトのpostMessage()メソッドが呼ばれたとき、
UAはPresentationSessionを通してメッセージをポストするためのアルゴリズムを実行しなければならない。

<!-- 
When the close() method is called on a PresentationSession, the user agent must run the algorithm to close a presentation session.
 -->

PresentationSessionオブジェクトのcloseメソッドが呼ばれたとき、UAはpresentation sessionをクローズするためのアルゴリズムを実行しなければならない。

#### 6.3.1 PresentationSessionを通してのメッセージの送信

#### 6.3.2 PresentationSessionを使ったメッセージの受信

#### 6.3.4 PresentationSessionのクローズ

#### 6.3.4 イベントハンドラ

以下のイベントハンドラ(と対応するイベントハンドライベントタイプ)は、PresentationSessionインタフェースを実装するオブジェクトで、イベントハンドラIDL属性としてサポートされなければならない。

Event handler | Event handler event type
---- | ---
onmessage | message
onstatechange | statechange


## 7 NavigatorPresentationインタフェース

### 7.1 プレゼンテーションセッションの開始

### 7.2 プレゼンテーションセッションへの参加

### 7.3 プレゼンテーションコネクションの確立

### 7.4 onavailablechangeイベントハンドラ

### 7.5 onavailablechangeへのイベントハンドラの追加

### 7.6 イベントハンドラの削除

### 7.7 使用可能プレゼンテーションディスプレイのリストの監視

## 8 AvailableChangeEventインタフェース

```
[Constructor(DOMString type, optional AvailableChangeEventInit eventInitDict)]
interface AvailableChangeEvent : Event {
  readonly attribute boolean available;
};

dictionary AvailableChangeEventInit : EventInit {
  boolean available;
};

```

availablechangeイベントは、プレゼンテーションディスプレイ



<!-- ### 6.2 PresentationSessionインタフェース -->


<!-- 
 -->