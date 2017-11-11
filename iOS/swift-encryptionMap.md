# 문서 암호화

- 문장의 모음을 바꿔 문장을 암호화 하는 프로그램

### 암호화 사전
```swift
let encryptionMap: [Character: Character] = [
    "a": "e",
    "e": "i",
    "i": "o",
    "o": "u",
    "u": "y",
    "y": "a"
]
``` 

### 암호화 함수
```swift
// 암호 문장
let setenceToEncrypt = "What’s coming will come and we’ll just have to meet it when it does."

// 암호화 문자열 
let encryptedResult = ""

var charArray = [Character](setenceToEncrypt)

for i in charArray{
    if let char = encryptionMap[i] { // char는 encryptionMap의 value값이 true면
        encryptedResult.append(char) // char를 encryptedResult 추가 
    }else {
        encryptedResult.append(i)   // encryptionMap의 value값 false면 i를 추가
    }
}

print(encryptedResult)
```

### 해독 함수
```swift
// 해독 함수 사전
var decryptionMap: [Character: Character] = [:]

for (key, value) in encryptionMap {
    decryptionMap[value] = key
}

var decryptedResult = ""

var decrypArray = [Character](encryptedResult)

for j in decrypArray {
    if let char = decryptionMap[i] {
        decryptedResult.append(char)
    }else {
        decryptedResult.append(j)
    }
}

print(decryptedResult)

```