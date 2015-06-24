    ԭ�� [Android M &quot;App Links&quot; implementation in depth](https://chris.orr.me.uk/android-app-linking-how-it-works/)&nbsp;

	At Google I/O 2015, a [new feature](http://www.androidpolice.com/2015/05/28/io-2015-android-m-will-support-app-deep-linking-without-that-annoying-selector-prompt/) was announced that allows "app developers to associate an app with a web domain they own."  This is intended to minimise the number of times a user sees the "Open with" dialog to choose which app, among those that can handle a certain URL, should be used to open a link.
    �ȸ�2015���I/O�����������һ��[������](http://www.androidpolice.com/2015/05/28/io-2015-android-m-will-support-app-deep-linking-without-that-annoying-selector-prompt/)&nbsp;���������߽�app�����ǵ�web������������һ�ٴ���Ϊ����С���û��������򿪷�ʽ���Ի���ĸ��ʡ�
    For example, with two Twitter apps installed �� the official client, and [Falcon Pro](https://play.google.com/store/apps/details?id=com.jv.materialfalcon) �� when you click on a Twitter URL from somewhere, you will currently see a dialog like this:
    ���磬���ǰ�װ������TwitterӦ�� - �ٷ��ĺ�[Falcon Pro](https://play.google.com/store/apps/details?id=com.jv.materialfalcon)��������ĳ���ط������Twitter URL��ʱ����ῴ�����µĶԻ���

    ![Clicking a Twitter link in the Android messaging app](/uploads/20150608/1433755603371966.png) ![app-links-intent-chooser-dialog.png](/uploads/20150608/1433755732968129.png "1433755732968129.png")
	With Android M �� and with an app that has explicitly opted-in to App Linking �� this dialog will no longer be shown.  Clicking on a link will open the _official app_ immediately, without prompting the user; there will be no chance to use a third-party app, or even a browser.
    �����ڰ�׿M�У����һ��app��ȷ��ָ����App����-����Ի��򽫲������ڡ����һ�����ӽ������򿪹ٷ���app��û�е�����app�Ļ��ᣬ�������һ���������

	In this example, when you click on that link, the Android system checks whether any of the apps that can handle twitter.com URLs have auto-linking enabled.  The system then verifies with twitter.com which app(s) may handle links for that domain, so that we can avoid prompting the user.
	����ͼ�������У����������Ǹ����ӣ���׿ϵͳ�����Ƿ���һ��app���Դ���twitter.com URL��Ȼ���twitter.com�˶��ĸ�app��s�����Դ�������������ӣ��������Ǿ��ܱ���Ӱ���û���
	Note that Android does not actively verify these links on demand, so there is no blocking on the network before Android decides which app to use.  More about that later.
    ע�ⰲ׿�������ڵ�����ӵ�ʱ��ź˶���Щ���ӣ�����ڰ�׿����ʹ���ĸ�app֮ǰ�����������������������������и������ۡ�
	While this will make Android more convenient �� in the majority of cases, you do want clicking a link to open the most appropriate app �� it seems bad for people who prefer to use third-party apps.  But note that this behaviour can be turned off from the system settings in Android M, on a per-app basis.
    ��Ȼ���ʹ��׿������- ��������£���ȷʵϣ�����һ�����Ӵ򿪵�������ʵ��Ǹ�app- ���Ƕ�����Щϲ��ʹ�õ�����app������˵���ƺ���һ�����¡�����������Ϊ������Android M��ϵͳ�����йص���

## 
    app���������ʵ��App Links

	The basic implementation details can be found on the [Android Developers' Preview Site](https://developer.android.com/preview/features/app-linking.html).

    ʵ�ֵ�ϸ�ڿ�����[Android Developers&#39; Preview ��վ](https://developer.android.com/preview/features/app-linking.html)���ҵ���
	If you have an app that handles links for, say, `example.com`, you must:
    �������һ����Ҫ�������ӣ�����example.com����app����Ӧ�ã�
*   Have the ability to upload files to the root of `example.com`

        *   Without this, you can't have your app automatically be the default for opening these links

*   Update your `build.gradle` with `compileSdkVersion 'android-MNC'`
*   Add the attribute `android:autoVerify="true"` to each `&lt;intent-filter&gt;` tag that contains `&lt;data&gt;` tags for HTTP or HTTPS URLs


    .���ϴ��ļ���example.com��·����Ȩ�ޣ����û�У����޷������app��Ϊ��Щ���ӵ�Ĭ�ϴ򿪷�ʽ��

    .��build.gradle�ļ�������compileSdkVersion &#39;android-MNC&#39;��

    .Ϊÿ��������&lt;intent-filter&gt;��ǩ

<pre class="brush:js;toolbar:false">&lt;intent-filter&gt;
    &lt;data android:scheme=&quot;http&quot; /&gt;
    ...
&lt;/intent-filter&gt;</pre>

    �������android:autoVerify=&quot;true&quot;��httpҲ������https��

    ע�������filter��д����ȥ�˺ܶ࣬��ʵ������������Ǹ����ӣ���������ʾ����

<pre class="brush:js;toolbar:false">&lt;activity ...&gt;
    &lt;intent-filter android:autoVerify=&quot;true&quot;&gt;
        &lt;action android:name=&quot;android.intent.action.VIEW&quot; /&gt;
        &lt;category android:name=&quot;android.intent.category.DEFAULT&quot; /&gt;
        &lt;category android:name=&quot;android.intent.category.BROWSABLE&quot; /&gt;
        &lt;data android:scheme=&quot;http&quot; android:host=&quot;www.android.com&quot; /&gt;
        &lt;data android:scheme=&quot;https&quot; android:host=&quot;www.android.com&quot; /&gt;
    &lt;/intent-filter&gt;
&lt;/activity&gt;</pre>
	Verification is done per hostname and not per intent filter, so you don't _technically_ need to add this attribute to each tag if you have multiple, but it doesn't hurt.
    ��֤����������Ϊ��λ�Ķ�������intent filterΪ��λ�ģ���˴Ӽ����Ͻ���������ҪΪÿ����ǩ����Ӹ����ԣ����������Ҳ�����ʲô���⡣

### 
    ����JSON�ļ�
	To allow Android to verify that your app should be allowed to use the app linking behaviour, you need to supply a JSON file with the application ID and the public certificate fingerprint of the APK(s) in question.
    Ϊ�����ð�׿������֤�����app��Ҫ������ʹ��app������Ϊ��Ϊ�ˣ���Ҫ�ṩһ��JSON�ļ���JSON�ļ�����Ҫ����app��ID�Լ�APK�Ĺ�Կ֤�顣
	The file must contain a JSON array with one or more objects, one per app ID you wish to verify:
    ����ļ��������һ��JSON���飬�����п�����һ�����߶������ÿ�������Ӧһ��������֤��app ID:

<pre class="brush:js;toolbar:false">[
  {
    &quot;relation&quot;: [&quot;delegate_permission/common.handle_all_urls&quot;],
    &quot;target&quot;: {
      &quot;namespace&quot;: &quot;android_app&quot;,
      &quot;package_name&quot;: &quot;com.example.myapp&quot;,
      &quot;sha256_cert_fingerprints&quot;: [&quot;6C:EC:C5:0E:34:AE....EB:0C:9B&quot;]
    }
  }
]</pre>

    For example, if you have a `com.example.myapp` release version, and a `com.example.myapp.beta` beta version, you can allow both to be verified by specifying two objects in the array, each with the respective application ID and certificate values.
	���磬��������һ��com.example.myapp�ķ��а�app��ͬʱ����һ����com.example.myapp.beta��beta��app�������ͨ�������������������������������߶����Խ�����֤��ÿ��������и��Ե�app ID�͹�Կֵ��

    Note that the validation of this file is **very strict**: every object in the array must look exactly like the one here.  Adding any other objects to the array, or additional properties to an object will cause validation to fail entirely �� even if the object for the app in question is valid.    ע�⣬����ļ�����֤�Ƿǳ��ϸ�ģ������е�ÿ�����󶼱����������Ǹ�һģһ��������������������������Ķ��󣬻��߶������ж�������Զ��ᵼ��������֤ʧ�ܡ�- �������app��������Ч�ġ�

    It seems that you can specify multiple SHA256 certificate fingerprints per app, in case you sign the same build with different keys for some reason.  In any case, this fingerprint can be obtained via:	
    Ϊ�˷�ֹΪͬһ������ע���˲�ͬ��key��ò��һ��app����ָ�����SHA256ָ��֤�飬����������ָ��֤�����ͨ�����·�ʽ��ã�

<pre class="brush:js;toolbar:false">echo | keytool -list -v -keystore app.keystore 2&gt; /dev/null | grep SHA256:</pre>

### 
    �ϴ�JSON�ļ�
	Having created this file, you must upload it and ensure it's available at the verification URL: `http://example.com/.well-known/statements.json`.
    �������ļ�֮������Ҫ�ϴ���ͬʱ��֤������ʹ�����URL����[http://example.com/.well-known/statements.json��](http://example.com/.well-known/statements.json.)
	Currently this URL **must** be accessible via HTTP; the final M release will **only** attempt to access the URL via HTTPS.
    Redirects to HTTPS, or any other redirect (whether via HTTP status codes 301, 302 or 307) seem to be ignored and treated as a failure in the first M preview release.
    Ŀǰ���URL��http�ģ����յ�M�汾��ֻ����ͨ��HTTPS���ʸ�URL���ڵ�һ��MԤ�����У��ض���HTTPS�������κ������ض���301��302����307��ò�ƶ��ᱻ���Բ�����Ϊʧ�ܡ�
    The scheme of the verification URL is also independent from the `android:scheme` values in your `&lt;intent-filter&gt;` tags. Even if you have a filter that only accepts HTTPS URLs, the verification URL still needs to be available via HTTP.	
    URL��scheme��&lt;intent-filter&gt;��ǩ�е�android:schemeֵ�ǻ�����ɵģ���ʹ����һ��ֻ����HTTPS URLs��filter����֤URL��Ȼ��Ҫͨ��HTTP���ʡ�
	After we've taken a look at how Android uses this information, we'll see how this process can be debugged.
    ���˽��˰�׿���ʹ����Щ��Ϣ֮���������������debug������̡�

## 
    Android M�����ʵ��App Links��
	App link verification involves two components in the Android system: the Package Manager and an Intent Filter Verifier.
    App������֤�漰����׿ϵͳ�������齨��Package Manager��Intent Filter Verifier��
	**PackageManager** is the standard component that has always been around �� it takes care of verifying that APKs for installation are valid, granting permissions to apps, and otherwise being the source of truth about what's installed on the system.
    **PackageManager**��һ���޴����ڵı�׼�齨 - ��������֤����װ��apk�Ƿ���Ч������appȨ�ޣ����⻹����ͨ����֪��ϵͳ�ϰ�װ��Щʲôapp��
	New in Android M is the **Intent Filter Verifier**.  This is a component responsible for fetching the app link verification JSON, parsing it, validating it, and reporting back to PackageManger.
    ��**Intent Filter Verifier**����Android M�ϲ��е��������������齨�����ȡ����ָ���JSON��֤������������֤����Ȼ�󽫱��淵�ظ�PackageManger��
	 It appears that there can only be one Intent Filter Verifier active on the system, though this component isn't something that users can easily replace �� in order to register as a verifier, the `android.permission.INTENT_FILTER_VERIFICATION_AGENT` permission is required, which is only available to apps signed with the system key.
	��Ȼ����齨�����û����׾����滻�ģ����ƺ�ϵͳ��ֻ����һ���״̬��Intent Filter Verifier - ��Ҫע���Ϊһ��verifier������Ҫ��android.permission.INTENT_FILTER_VERIFICATION_AGENTȨ�ޣ������Ȩ��ֻ��ǩ����ϵͳ��Կ��app���ܵõ���
	You can see the current active intent filter verifier via the command:
    �����ͨ�����µ�����鿴��ǰ�����intent filter verifier��

<pre class="brush:js;toolbar:false">adb shell dumpsys package ifv</pre>
    In the first M preview release, `com.android.statementservice` fulfils this role.
    �ڵ�һ��MԤ�����У�com.android.statementservice��ȫ�����������ɫ��

### 
    How App Links are verified
	
    App link verification is done once �� at install time.  This is done so that, as mentioned above, the system does not need to block on the network every time you click on a link.
    App������֤�ڰ�װ��ʱ���һ������ɡ������Ϊʲô�ո�����˵������ÿ�ε�����ӵ�ʱ���������硣

    ![](/uploads/20150608/1433755846674264.png)
 When a package is installed, or an existing package is updated:
    ��һ��package��װ��ʱ�򣬻������е�package������ʱ��
1.  PackageManager does its usual validation of the incoming APK
2.  If successful, the package will be installed, and a broadcast intent with the action `android.intent.action.INTENT_FILTER_NEEDS_VERIFICATION` is sent, along with the installed package info
3.  The Intent Filter Verifier has a broadcast receiver which picks this up
4.  A list of _unique hostnames_ is compiled from the `&lt;intent-filter&gt;` tags in the package
5.  The verifier attempts to fetch `statements.json` from each unique hostname
6.  Every JSON file fetched is checked for the application ID and certificate of the installed package
7.  If (and only if) **all** files match, then _success_ is signalled to PackageManager; otherwise _failure_
8.  PackageManager stores the result

    1.PackageManager�Լ�����װ��apk���������֤��

    2.����ɹ������package������װ��ͬʱ����һ������android.intent.action.INTENT_FILTER_NEEDS_VERIFICATION�Ĺ㲥intent��intent�л�Я���и�package����Ϣ��

    3.Intent Filter Verifier�Ĺ㲥����������ȡ����㲥��

    4.��package��&lt;intent-filter&gt;��ǩ�б����һ���������������б�

    5.verifier���Դ�ÿ�����е��������л�ȡstatements.json��

    6.ÿһ������ȡ��JSON�ļ�����������application ID�Ͱ�װ����֤�顣

    7.ֻ�е������ļ�ͬʱ����ʱ���Żᷢ�ͳɹ���Ϣ��PackageManager������ʧ�ܡ�

    8.PackageManager�洢�����

    If verification fails, app link behaviour will not be available to your app until verification succeeds �� your app will appear in the "Open with" dialog as usual (unless another app has passed verification for the same hostname).	
    �����֤ʧ�ܣ�app���ӽ��޷�ָ�����app - ���app��������һ�������ڡ��򿪷�ʽ���Ի����У�������һ��appͨ����ͬһ��������֤����
    As far as I can tell, verification is only attempted once per install or upgrade, so the next chance to pass verification for most users will be when they next upgrade your app.
    �������˽�Ķ��ԣ���ֻ֤���ڰ�װ��������ʱ��ᷢ������˶Դ�����û���˵���ٴ�ͨ����֤�Ļ�������app��һ��������ʱ��

### 
    Intent Filter Verifier����Ϊ
	**Hostnames**
    **������**

    Note that `example.com` and `www.example.com` are treated as separate hostnames.  This means your `statements.json` must be reachable directly at both hostnames.	
    example.com��www.example.com�ᱻ��Ϊ�����������������������Ҫ��statements.json�������������¶��ǿ�ֱ��ġ�
    For example, if you automatically redirect all requests to `http://www.example.com/*` to `https://example.com/*`, then this will cause verification to fail for this one hostname, and therefore app link verification as a whole will fail.
    ���磬����㽫���е������ض���http://www.example.com/* to https://example.com/*��������������������֤ʧ�ܣ��Ӷ���������app������֤ʧ�ܡ�
    In this case, you would have to add special cases to your web server configuration to ensure that every request for `statements.json` directly returns an HTTP 200.  Possibly this rule will be relaxed in future releases.
    ��������£��������ҪΪ���web����������������ã�ȷ��ÿ������statements.json����������ֱ�ӷ���HTTP 200�����Լ���ں���İ汾�п��ܻ��������ɡ�

    **��Ӧʱ��**
    If the verifier cannot create a connection to your web server and receive an HTTP response within five seconds, verification will fail.
    ���verifier������5��֮�ں����web�������������Ӳ����յ�HTTP��Ӧ����֤��ʧ�ܡ�

    **Lack of connectivityȱ�����ӻ���**
    Likewise, if the device is offline when verification starts, or has a bad connection, verification will fail.
    ͬ���ģ��������֤��ʼ��ʱ���豸�����ߵģ��������绷���ܲ��֤Ҳ��ʧ�ܡ�

    **HTTP ����**
    The current implementation of the Intent Filter Verifier respects regular HTTP caching rules for the most part.
    ĿǰIntent Filter Verifier��ʵ�ֻ�����ѭHTTP�������
    If your `statements.json` response contains a `Cache-Control: max-age=[seconds]` header, the response will be cached on disk by the verifier.  Though `max-age` values under 60 seconds are ignored; in fact 60 seconds seem to be _added_ to all `max-age` values (for some reason).  Similarly, an `Expires` header is also respected when caching responses.
    If your statements.json response contains a Cache-Control: max-age=[seconds] header, the response will be cached on disk by the verifier. &nbsp;Though max-age values under 60 seconds are ignored; in fact 60 seconds seem to be _added_ to all max-age values (for some reason). &nbsp;Similarly, an Expires header is also respected when caching responses.

    ������statements.json��Ӧ������Cache-Control: max-age=[seconds]ͷ������ô�����Ӧ����verifier���浽���̡�

    If you have ETag or Last-Modified headers, 
then the verifier will attempt to make a conditional request using these
 values the next time it attempts to verify the corresponding hostname.
Though from what I&#39;ve seen, if these headers exist but without explicit 
cache control headers, the response will be cached for a possibly 
indefinite length of time.

    Cache headers are only respected for HTTP 200 responses. &nbsp;If you have
 a 404 response with any sort of cache expiry headers, these will be 
ignored the next time the verifier needs to contact your hostname.

### 
    Debugging App Links
    When the Android system attempts to verify your app link setup, there is little feedback aside from a true/false value reported by `PackageManager` to logcat, e.g.:
    ����׿ϵͳ��ͼ��֤���app����ʱ��PackageManager������logcat�б���true/falseֵ֮�⣬����һЩ�������������磺

<pre class="brush:js;toolbar:false">IntentFilter ActivityIntentInfo{1a61a0a com.example.myapp/.MainActivity}
 verified with result:true and hosts:example.com www.example.com</pre>
	However, you can ask the system at any time for the app linking verification status of your package:
    ���ǣ����������ʱ����ϵͳ��ѯpackage��app������֤״̬��

<pre class="brush:js;toolbar:false">adb shell dumpsys package d</pre>
	This will return a list of verification entries like this:
    ��᷵�����µ���֤��Ŀ��Ϣ��

<pre class="brush:js;toolbar:false">Package Name: com.example.myapp
Domains: example.com www.example.com
Status: always</pre>
    There may be multiple entries for your package: one for the system, and zero or more for users on the system, whose preferences override the system value.
    ���ܻ��ж��������package����Ŀ��һ����ϵͳ�ģ�������߶���û���- ��Щ�û���ƫ�ø�����ϵͳ������
    The possible status values seem to be:
    ���ܻ������״ֵ̬�������£�
	
*   **undefined** �� apps which do not have link auto-verification enabled in their manifest
*   **ask** �� apps which have failed verification (i.e. _ask_ the user via the "Open with" dialog)
*   **always** �� apps which have passed verification (i.e. _always_ open this app for these domains)
*   **never** �� apps which have passed verification, but have been disabled in the system settings

*   **undefined** ��&nbsp; appû����manifest�����������Զ���֤���ܡ�

*   **ask** �� app��֤ʧ�ܣ���ͨ���򿪷�ʽ�Ի���ѯ���û���

*   **always** �� appͨ������֤���������������Ǵ����app��

*   **never** �� appͨ������֤������ϵͳ���ùر��˴˹��ܡ�

    If you didn't manage to pass verification, you can retry by simpling reinstalling the same version, e.g.:
    �����û��ͨ����֤��������ٴγ������°�װͬһ�汾��

<pre class="brush:js;toolbar:false">adb install -r app/build/outputs/apk/app-debug.apk</pre>

    If you can't see the `statements.json` URL being requested on your web server at install time, you can clear out the Intent Verifier Service HTTP cache, so that next time it will have to hit the server:
    ������ڰ�װ��ʱ��û���ڷ������Ͽ���statements.json URL��������������Intent Verifier�����HTTP���棬�����´ξͻ�ֱ�������������

<pre class="brush:js;toolbar:false">adb shell pm clear com.android.statementservice</pre>


If you have doubts about whether the `statements.json` contents are returned correctly, you can use the `-tcpdump` option of the Android emulator to check exactly what's being sent over the network �� though note this won't work so simply once the final M release is out and encryption is required.

Alternatively you can use the `-http-proxy` option of the emulator and pass all network requests through a proxy like [Charles](http://charlesproxy.com/).
    ����㲻֪��statements.json�������Ƿ���ȷ���أ�����ʹ�ð�׿ģ������-tcpdumpѡ������������Ϸ��͵���ʲô - ע�ⰲ׿M���հ����֮���û��ô�����ˣ���Ϊ�����Ǽ��ܵġ�

    ����һ��ѡ���Ǿ���ʹ��ģ������-http-proxyѡ������е���������ͨ����������[Charles](http://charlesproxy.com/)����

## 
    �ܽ�

Although I was initially sceptical about App Links due to a perceived takeover of the existing, amazing intent system of Android, I'm glad to see that it should help in most cases, and there is a very simple way to turn this off in the cases where users don't like it.

Given that the JSON parsing and HTTP request behaviour are remarkably strict in the verifier service, hopefully some of the detailed information here will help you with your implementation of app linking.

Good luck!	
    ��Ȼ�ڿ�ʼ�ҵ���App Links��ȡ��Ŀǰ��׿�Ϸǳ����intent���ƣ�����Ҳ���ڿ�������ڴ��������������õģ���������û���ϲ����ʹ�ü򵥵ķ����Ϳ��԰����ص���

    ���ǵ�verifier�����JSON������HTTP�����쳣�ϸ�ϣ���������ᵽ��һЩϸ�ڶ���ʵ��app linking����������ף����ˣ�

    