# Homepwner

<img src=“/images/homepwner.png” width=“300”>

### itemViewController
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