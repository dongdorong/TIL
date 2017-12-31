# Firebase Install
- 어플과 서버를 연동

### 파일추가

- 해당하는 폴더에 GoogleService-Info.plist 파일 추가

### terminal

```
해당 앱 폴더를 Terminal app 아이콘으로 drag & drop:: 터미널이 이 폴더 안에서 열림
```

#### terminal app
```
//cocoapods 다운로드

sudo gem install cocoapods


pod init


open .
```

```
//Podfile 파일을 X-code를 통해서 열기
// 3line

pod 'Firebase/Core'

```

#### terminal app
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

### X-code Project 다시 실행

<img src="/img/firebase.png" width="200">

`이 아이콘으로 된 프로젝트로 재 실행하여 firebase 사용한다.`

### Firebase에서 Database를 사용 할 수 있도록 설정
```
Pods > Podfile 경로

pod 'Firebase/Database'

추가해야 import FirebaseDatabase를 사용할 수 있다.
```

#### terminal app

- 해당하는 폴더에 GoogleService-Info.plist 파일 추가

```
pod install
```

# FIRDatabaseReference -> DatabaseReference
- ref: DatabaseReference :: nil 일때 자꾸 빨간불이 들어온다면? [링크 참조](https://firebase.google.com/support/guides/firebase-ios)

```
//데이터베이스 코드 업데이트

pod update 
```
