# NoteApp
- todo리스트와 같은 간단한 정보를 입력할 수 있는 App
- UserDefault:: 간단한 정보를 입력할 때 사용
- UIAlert:: 알럿으로 정보 저장
- UITableViewCellEditingStyle:: 테이블을 지울때 애니메이션 추가

<img src="/img/note-alert.png" width="300">
<img src="/img/note-main.png" width="300">


```swift
import UIKit

class ViewController: UIViewController, UITableViewDataSource, UITableViewDelegate {
    
    let dataKey = "dataKey"
    
    @IBOutlet weak var myTableView: UITableView!
    
    var item:[String] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        if let arrayData = UserDefaults.standard.object(forKey: dataKey) as? [String]{
            item = arrayData
        }
        
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    @available(iOS 2.0, *)
    public func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int{
        return item.count
    }
    
    @available(iOS 2.0, *)
    public func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell{
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
        cell.textLabel?.text = item[indexPath.row]
        return cell
    }

    // 저장
    @IBAction func addItem(_ sender: UIBarButtonItem) {
        let alert = UIAlertController(title: "New Notes", message: "please add note", preferredStyle: .alert)
        
        alert.addTextField { (textField: UITextField) -> Void in }
        
        let saveAction = UIAlertAction(title: "save", style: .default) { (action: UIAlertAction) -> Void in
            let newMemo = alert.textFields!.first!
            if newMemo.text != ""{
                self.item.append(newMemo.text!)
                UserDefaults.standard.set(self.item, forKey: self.dataKey)
            }
            self.myTableView.reloadData()
        }
        
        let cancelAction = UIAlertAction(title: "cancel", style: .default) { (action: UIAlertAction) -> Void in }
        
        alert.addAction(cancelAction)
        alert.addAction(saveAction)
        
        present(alert, animated: true, completion: nil)
        
    }
    @available(iOS 2.0, *)
    public func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath){
        if editingStyle == .delete {
            item.remove(at: indexPath.row)
            UserDefaults.standard.set(item, forKey: dataKey)
            tableView.deleteRows(at: [indexPath], with: .fade)
        }
    }
    
}
```