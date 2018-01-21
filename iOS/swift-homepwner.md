# Homepwner

<img src="/img/homepwner.png" width="300">

### itemViewController.swift
```swift

import UIKit

class ItemViewController: UITableViewController{

    var itemStore: ItemStore!

    override func viewDidLoad() {
        super.viewDidLoad()

        // 유동적인 높이의 Cell을 만들때
        tableView.rowHeight = UITableViewAutomaticDimension
        tableView.estimatedRowHeight = 65

    }

    override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)

        tableView.reloadData()
    }

    override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return itemStore.allItems.count
    }

    override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {

        let cell = tableView.dequeueReusableCellWithIdentifier("ItemCell", forIndexPath: indexPath) as! ItemCell

        // 텍스트 크기 업데이트
//        cell.updateLabel()

        let item = itemStore.allItems[indexPath.row]

        cell.nameLabel.text = item.name
        cell.serialNumberLAbel.text = item.serialNumber
        cell.valueLabel.text = "$\(item.valueInDollers)"

        return cell
    }

    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
        if segue.identifier == "ShowItem" {

            // 방금 누른 행
            if let row = tableView.indexPathForSelectedRow?.row {

                // 연결된 아이템을 갖고 온다
                let item = itemStore.allItems[row]
                let detailViewController = segue.destinationViewController as! DetailViewController
                detailViewController.item = item
            }

        }
    }


    @IBAction func addNewItem(sender: AnyObject) {

        // 새 물품을 만들고 그것을 저장소에 추가한다
        let newItem = itemStore.creatItem()

        // 배열 안에서 이 항목의 위치를 계산
        if let index = itemStore.allItems.indexOf(newItem) {
            let indexPath = NSIndexPath(forRow: index, inSection: 0)

            // 테이블에 새로운 행 삽입
            tableView.insertRowsAtIndexPaths([indexPath], withRowAnimation: .Automatic)
        }

    }

    required init?(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)

        navigationItem.leftBarButtonItem = editButtonItem()
    }

    override func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
        if editingStyle == .Delete {
            let item = itemStore.allItems[indexPath.row]

            let title = "Delete \(item.name)?"
            let message = "삭제하시겠습니까?"

            let ac = UIAlertController(title: title, message: message, preferredStyle: .ActionSheet)

            let cancelAction = UIAlertAction(title: "Cancel", style: .Cancel, handler: nil)
            let deletAction = UIAlertAction(title: "Delete", style: .Destructive, handler: { (action: UIAlertAction) -> Void in
                self.itemStore.removeItem(item)
                self.tableView.deleteRowsAtIndexPaths([indexPath], withRowAnimation: .Automatic)
            })

            ac.addAction(cancelAction)
            ac.addAction(deletAction)

            presentViewController(ac, animated: true, completion: nil)

        }
    }


}

```


### DetailViewController.swift
```swift

import UIKit

class DetailViewController: UIViewController, UITextFieldDelegate {

    @IBOutlet weak var nameField: UITextField!
    @IBOutlet weak var serialField: UITextField!
    @IBOutlet weak var valueField: UITextField!
    @IBOutlet weak var dateLabel: UILabel!

    var item: Item! {
        didSet{
            navigationItem.title = dateFormatter.stringFromDate(item.dateCreated)

            // item name
//            navigationItem.title = item.name
        }
    }

    let numberFomatter: NSNumberFormatter = {
        let formatter = NSNumberFormatter()
        formatter.numberStyle = .DecimalStyle
        formatter.minimumFractionDigits = 2
        formatter.maximumFractionDigits = 2
        return formatter
    }()

    let dateFormatter: NSDateFormatter = {
        let formatter = NSDateFormatter()
        formatter.dateStyle = .MediumStyle
        formatter.timeStyle = .NoStyle
        return formatter

    }()

    override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)

        view.endEditing(true)

        nameField.text = item.name
        serialField.text = item.serialNumber
        valueField.text = numberFomatter.stringFromNumber(item.valueInDollers)
        dateLabel.text = dateFormatter.stringFromDate(item.dateCreated)

    }

    override func viewWillDisappear(animated: Bool) {
        super.viewWillDisappear(animated)

        // Clear first responder
        view.endEditing(true)

        // 변경 사항을 Item에 저장
        item.name = nameField.text ?? ""
        item.serialNumber = serialField.text

        if let valueField = valueField.text, value = numberFomatter.numberFromString(valueField){
            item.valueInDollers = value.integerValue
        } else {
            item.valueInDollers = 0
        }

    }

    func textFieldShouldReturn(textField: UITextField) -> Bool {

        // Return을 터치하면 키보드 종료
        textField.resignFirstResponder()
        return true
    }

    @IBAction func backgroundTap(sender: UITapGestureRecognizer) {

        // view를 터치하면 editind 종료
        view.endEditing(true)
    }

}


```


### item.swift
```swift

import UIKit

class Item: NSObject{

    var name: String
    var valueInDollers: Int
    var serialNumber: String?
    let dateCreated: NSDate

    init(name: String, serialNumber: String?, valueInDollers: Int) {
        self.name = name
        self.serialNumber = serialNumber
        self.valueInDollers = valueInDollers
        self.dateCreated = NSDate()

        super.init()
    }

    convenience init(random: Bool = false) {
        if random {
            let adjective = ["Dona", "iOSDeveloper", "UIDesigner"]
            let nouns = ["Cat", "Youtuber", "Application"]

            var idx = arc4random_uniform(UInt32(adjective.count))
            let randomAdjective = adjective[Int(idx)]

            idx = arc4random_uniform(UInt32(nouns.count))
            let randomNoun = nouns[Int(idx)]

            let randomName = "\(randomAdjective) \(randomNoun)"
            let randomValue = Int(arc4random_uniform(100))
            let randomSerialNumber = NSUUID().UUIDString.componentsSeparatedByString("-").first!

            self.init(name: randomName, serialNumber: randomSerialNumber, valueInDollers: randomValue)


        }
        else{
            self.init(name: "", serialNumber: nil, valueInDollers: 0)
        }
    }

}


```

### itemCell.swift
```swift

import UIKit

class ItemCell: UITableViewCell {

    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var serialNumberLAbel: UILabel!
    @IBOutlet weak var valueLabel: UILabel!

    // 텍스트 크기 수동으로 변경
//    func updateLabel() {
//        let bodyFont = UIFont.preferredFontForTextStyle(UIFontTextStyleBody)
//        nameLabel.font = bodyFont
//        serialNumberLAbel.font = bodyFont
//
//        let captionFont = UIFont.preferredFontForTextStyle(UIFontTextStyleCaption1)
//        serialNumberLAbel.font = captionFont
//    }

}


```


### itemStore.swift

```swift

import UIKit

class ItemStore{

    var allItems = [Item]()

    func creatItem() -> Item {
        let newItem = Item(random: true)

        allItems.append(newItem)

        return newItem
    }

    // ItemStore 5개 까지 자동 입력

//    init() {
//        for _ in 0..<5{
//            creatItem()
//        }
//    }


    // 행삭제

    func removeItem(item: Item) {

        if let index = allItems.indexOf(item) {
            allItems.removeAtIndex(index)
        }
    }

}


```