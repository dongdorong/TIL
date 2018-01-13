# LargeTitle

<img src="/img/donaNotes.png" width="300">

- iOS 11 LargeTitle code
- NavigationController - TableviewController에 사용

```swift
override func viewDidLoad() {
        super.viewDidLoad()

        navigationController?.navigationBar.prefersLargeTitles = true
        navigationItem.title = "Dona Notes"
        navigationController?.setToolbarHidden(false, animated: false)   
}
```


# Tableview Controller Toolbar
- bottom toolbar code
- action:: 액션 추가해줄때 #selector로 function만들어서 추가

```swift
override func viewDidLoad() {
        super.viewDidLoad()

        let add = UIBarButtonItem(title: "test", style: .plain, target: self, action: nil)
        let spacer = UIBarButtonItem(barButtonSystemItem: .flexibleSpace, target: self, action: nil)
        let addT = UIBarButtonItem(title: "test", style: .plain, target: self, action: nil)
        
        toolbarItems = [add, spacer, addT]
        
}
```

# Tableview rowHeight Custom

```swift
override func viewDidLoad() {
        super.viewDidLoad()

 	tableView.rowHeight = 80       
}
```