# Firebase Install
- 어플과 서버를 연동

### 파일추가

- 해당하는 폴더에 GoogleService-Info.plist 파일 추가

### terminal

```
해당 앱 폴더를 Terminal app 아이콘으로 drag & drop:: 터미널이 이 폴더 안에서 열림
```

```
//cocoapods 다운로드

sudo gem install cocoapods
```

```
pod init
```

```
open .
```

```
//Podfile 파일을 X-code를 통해서 열기
// 3line

pod 'Firebase/Core'

```

```
pod install
```

### 해당 앱 AppDelegate.swift

```
import UIKit
import Firebase // 추가

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        
        FirebaseApp.configure() // 추가
        
        return true
}
```
