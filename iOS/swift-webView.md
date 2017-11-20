# WebView
- webview:: 인터넷 브라우저와 똑같은 기능

<img src="/img/webview.png" width="300">

```swift
@IBOutlet weak var webView: UIWebView!

override func viewDidLoad() {
    super.viewDidLoad()
    
    if let myUrl = URL(string: "http://www.naver.com") {
        
        let request = URLRequest(url: myUrl)
        
        webView.loadRequest(request)
    }

}
```
- optional type:: if let으로 안전하게 풀어준다.
- HTTP는 로드되는 방법으로 열어준다.

# App Transport Security
- apple이 정한 네트워크 보완 기준

### info.plist file open
- 마우스 오른쪽 Add row 추가
- App Transport Security Settings
    * Allow Arbitrary Loads:: HTTP site는 모두 열어준다.
    * Exception Domains(Dictionary):: 정해준 site만 열어준다.
        * naver.com(Dictionary):: 도메인 설정
            * NSTemporaryExceptionAllowsInsecureHTTPLoads(Boolean):: YES
            * NSIncludesSubdomains(Boolean):: YES      