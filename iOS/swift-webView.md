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
            
# HTML 적용 방법

### loadHTMLString

```swift
webView.loadHTMLString("<div><b>Hello, Dona :)</b></div>", baseURL: nil)
```

# HTML 다운로드 방법
```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view, typically from a nib.
    
    if let myUrl = URL(string: "https://github.com/dongdorong") {

        let request = URLRequest(url: myUrl)
        
        let task = URLSession.shared.dataTask(with: request){
            data, response, error in
            
            // 에러가 있는지 확인 
            if error != nil{
                print("warning!!")
            } else {
                if let unwarppedData = data {
                    if let dataString = String(data: unwarppedData, encoding: String.Encoding(rawValue: String.Encoding.utf8.rawValue)){
                        print(dataString)
                    }
                }
            }
        }
        
        task.resume()
        
    }
}
```
- dataTask:: 정보를 받아오는 일을 뜻함 -> request에서 해당하는 정보를 받아온다.
- task를 실행하면 dataTask에서 자동으로 3가지 값을 받아온다. (클로저)
    1. data:: URL에서 받아오는 HTML이 담아져 있다.
    2. response:: 서버의 응답
    3. error:: request가 false일 때    
- 가장 먼저 error가 있는지 체크한다.
- 오류가 없을때,
    1. data는 nil이 나올 수 있기 때문에 optional type -> if let으로 안전하게 풀어준다.
    2. data를 풀어온 값인 unwarppedData를 utf8 인코딩 방식으로 이용해서 String으로 형변환해준다.
    3. 문자열로 형변환이 안되면 nil 값이 넘어 오기 때문에 if let으로 풀어준다.
    4. task 실행 -> task.resume()