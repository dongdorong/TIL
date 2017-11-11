# 팔린드롬 (palindrome)
- 거꾸로 읽어도 똑같은 단어
- 단어가 팔린드롬이면 true 아니면 false를 리턴

```swift
var isPalindrome(word: String) -> Bool {
    let charArray = [Character](word)
    
    for i in 0...(charArray.count) / 2 { // 2로 나누는 이유는 센터 값을 중심으로 0값과 끝값 / 1값과 끝-1값이 같기 때문에
        if charArray[i] != charArray[Int(charArray.count - i - 1)] {
            return false
        }
    }
    return true
}
```
- Tip: 일반 문자열을 for문에 사용 가능하게 하려면 [Character] 문자배열로 만들면 된다.


### test 출력
```swift
print(isPalindrome(word: "racecar"))  // true
print(isPalindrome(word: "stars"))    // false
print(isPalindrome(word: "kayak"))    // true
print(isPalindrome(word: "hello"))    // false
```