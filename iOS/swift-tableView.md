# 테이블뷰 tableView

<img src="/img/tableView.png" width="300">

### 1. main storyboard에 tableView 생성

### 2. tableView를 viewController에 dataSource 연결

- viewController에 dataSource를 연결하는 이유: 이 tableView의 data를 viewController에서 가져 올 것을 명령

### 3. viewController.swift file에 [UITableViewDataSource] 프로토콜 추가

### 4. numberOfSections / numberOfRowInSection / cellForRowAt 메서드 추가

#### numberOfRowInSection
```swift
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return (리턴할 row의 개수)
}
```
- 각 section에 담을 항목의 개수 리턴 (줄 수를 리턴)
- 각 항목에 몇개의 아이템을 담을지 정해줄때 

#### cellForRowAt
```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell{
    
    // row의 Identifier를 정의
    let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    
    // row마다 들어갈 내용을 textLabel의 text property로 정의
    cell.textLabel?.text = "Section \(indexPath.section) Row \(indexPath.row)"
    
    return cell
}
```
- 각 줄에 들어갈 항목을 리턴
- 각 항목에 어떤 내용이 들어가는지 정해줄때
- **indexPath** 각 항목의 section, row 값을 보관

#### numberOfSections
```swift
func numberOfSections(in tableView: UITableView) -> Int{
    return (리턴할 section의 개수)
}
```
- section 개수 리턴
- 이 메서드를 등록하지 않으면 section이 자동으로 1만 리턴한다.
- 이 메서드는 **optional**  이기 때문에 정의를 안해줘도 된다.

### 5. tableView content -> Dynamin Prototypes

- Dynamin Prototypes:: program 실행중에 바뀔 수 있다.
- Static Cells:: 정해져 있는 값만 받는다.

### 6. tableViewCell의 identifier 정의

- cellForRowAt에서 정의해준 **withIdentifier: "Cell"** 의 [Cell]을 identifier로 정의해준다.

### 7. Array의 item을 tableView에 적용
```swift
// 배열 정의
var item: [String] = []

func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return item.count // item array의 항목 개수만큼 리턴
}

func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell{
    
    // row의 Identifier를 정의
    let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    
    // row마다 들어갈 내용을 textLabel의 text property로 정의
    cell.textLabel?.text = item[indexPath.row] // item 배열을 차례대로 적용
    
    return cell
}

```

### 8. Alert 창으로 item 추가
- 네비게이션 아이템을 IBOutlet 연결
```swift
@IBAction func addItem(_ sender: UIBarButtonItem) {

    // alert 생성
    let alert = UIAlertController(title: "새로운 메모", message: "새로운 메모를 입력해주세요.", preferredStyle: UIAlertControllerStyle.alert)
    
    // textField 추가
    alert.addTextField { (textField: UITextField) -> Void in
    }
    
    // 저장
    let saveAction = UIAlertAction(title: "저장", style: UIAlertActionStyle.default) { (action: UIAlertAction) -> Void in
        
        // nil 값이 있을 수 있기에 옵셔널
        let newMemo = alert.textFields!.first!
        
        // newMemo가 nil이 아닐때 item배열에 추가
        if newMemo.text! != "" {
            self.item.append(newMemo.text!)
        }
    }
    
     self.myTableView.reloadData() // item을 저장한다.
    
    // 취소
    let cancleAction = UIAlertAction(title: "취소", style: UIAlertActionStyle.default) { (action: UIAlertAction) -> Void in
    }
    
    // 취소, 저장 버튼 추가
    alert.addAction(cancleAction)
    alert.addAction(saveAction)
    
    // 알럿창 띄우기:: 좀 더 공부 필요
    present(alert, animated: true, completion: nil)

}
```
- **UIAlertControllerStyle.alert** 또는 **.alert** 로 쓰인다. swift가 자동으로 UIAlertControllerStyle를 인식한다. (87 line)

### 9. 삭제
1. tableView IBOutlet 생성
2. 스토리보드 tableView를 viewController의 delagate 연결
- delagate:: viewController에서 tableView의 터치, 스와이프 등 실행하겠다는 명령어
3. viewController에 UITableViewDelegate 추가
4. 사용하려는 프로토콜 추가
```swift
@IBOutlet weak var myTableView: UITableView!

// 어떤 행동을 취한 것인지 정해주는 프로토콜
public func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath){
    
    // 삭제
    if editingStyle == .delete {
        item.remove(at: indexPath.row) // 배열에 아이템 삭제
        tableView.deleteRows(at: [indexPath], with: UITableViewRowAnimation.fade) // 테이블뷰의 row삭제
    }
}
```