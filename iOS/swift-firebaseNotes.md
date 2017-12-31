# FirebaseNotes
- Console > Database > 규칙 true로 변경
```
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```


### ViewController.swift

```swift
import UIKit
import FirebaseDatabase

class NewTableViewCell: UITableViewCell{
    @IBOutlet weak var task: UILabel!
    @IBOutlet weak var date: UILabel!
}

class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource {
    
    @IBOutlet weak var tableView: UITableView!

    var ref: DatabaseReference? = nil
    
    var data:[ToDoItem] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        ref = Database.database().reference(withPath: "to-do-item")
        
        ref?.observe(.value, with: { (snapShot) in
            var newData: [ToDoItem] = []
            for item in snapShot.children {
                let snapShotItem = item as! DataSnapshot
                let snapShotValue = snapShotItem.value as! [String: AnyObject]
                
                let task = snapShotValue["task"] as! String
                let date = snapShotValue["date"] as! String
                
                let ref = snapShotItem.ref
                
                let newToDoItem = ToDoItem(task: task, date: date, ref: ref)
                
                newData.append(newToDoItem)
            }
            self.data = newData
            self.tableView.reloadData()
        })
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    @available(iOS 2.0, *)
    public func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int{
        return data.count
    }
    
    @available(iOS 2.0, *)
    public func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell{
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath) as! NewTableViewCell
        
        cell.task.text = data[indexPath.row].task
        cell.date.text = data[indexPath.row].date
        
        return cell
        
    }
    
    @IBAction func addItem(_ sender: UIBarButtonItem) {
        let alert = UIAlertController(title: "Note", message: "Please New Item", preferredStyle: .alert)
        alert.addTextField { (textField: UITextField) -> Void in }
        
        let saveAction = UIAlertAction(title: "Save", style: .default) { (action: UIAlertAction) -> Void in
            let newMemo = alert.textFields!.first!
            
            if newMemo.text != "" {
                let date = Date()
                let formatter = DateFormatter()
                
                formatter.dateFormat = "yy.MM.dd hh:mm:ss"
                
                let task = newMemo.text!
                let dateString = formatter.string(from: date)
                
                let newObject = ToDoItem(task: task, date: dateString, ref: self.ref!)
                
                self.data.append(newObject)
                
                let todoItemRef = self.ref!.child(task.lowercased())
                todoItemRef.setValue(newObject.toDictionary())
                
            }
            
//            self.tableView.reloadData()
        }
        
        let cancelAction = UIAlertAction(title: "Cancel", style: .cancel) { (action: UIAlertAction) -> Void in
            
        }
        
        alert.addAction(cancelAction)
        alert.addAction(saveAction)
        
        present(alert, animated: true, completion: nil)
        
    }
    
    @available(iOS 2.0, *)
    public func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
        if editingStyle == .delete {
            data[indexPath.row].ref?.removeValue()
        }
    }

}
```


### ToDoItem.swift

```swift
import Foundation
import FirebaseDatabase

class ToDoItem {
    let task: String
    let date: String
    
    var ref: DatabaseReference?
    
    // 초기화
    init(task: String, date: String, ref: DatabaseReference) {
        self.task = task
        self.date = date
        self.ref = ref
    }
    
    // Firebase 정보 저장 타입은 Any
    // 미리 바꿔주는 메서드 추가
    func toDictionary() -> Any{
        return[
            "task": task, // property를 value로 사전에 저장
            "date": date
        ]
    }
}
```