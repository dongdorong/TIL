# Login 
- 간단한 로그인 페이지를 만든다.
- Coredata 사용

<img src="/img/login-system.png" width="500">

## Login.xcdatamodeld

```
새로운 ENTITES 생성

User
```

```
Attributes 생성:: 저장할 정보들

id String
name String
password String
birth String
```

## ViewController

```swift
import UIKit
import CoreData

class ViewController: UIViewController {
    
    // LoginPageViewController에 넘길 변수 저장
    var user:NSManagedObject? = nil

    @IBOutlet weak var idField: UITextField!
    @IBOutlet weak var pwField: UITextField!
    
    // 알럿함수
    func displayErrorMessage(title: String, message: String){
        let alert = UIAlertController(title: title, message: message, preferredStyle: .alert)
        let confirm = UIAlertAction(title: "Confirm", style: .default) { (action: UIAlertAction) -> Void in
            
        }
        alert.addAction(confirm)
        present(alert, animated: true, completion: nil)
    }

    @IBAction func checkAndGo(_ sender: UIButton) {

        // 아이디 빈 문자열 오류 표시
        if idField.text! == ""{
            displayErrorMessage(title: "아이디를 입력해주세요.", message: "아이디를 다시 입력해주세요.")
            return
        }
        // 비밀번호 빈 문자열 오류 표시
        if pwField.text! == "" {
            displayErrorMessage(title: "비밀번호를 입력해주세요.", message: "비밀번호를 다시 입력해주세요.")
            return
        }
        
        let appDelegate = UIApplication.shared.delegate as! AppDelegate
        
        let context = appDelegate.persistentContainer.viewContext
        
	// NSFetchRequest로 User에 있는 값을 NSManagedObject로 리턴해서 값을 가져옴
        let request = NSFetchRequest<NSManagedObject>(entityName: "User")
        
        //원하는 값을 가져올때:: ID값을 가져옴
        request.predicate = NSPredicate(format: "id == %@", idField.text!)
        
	// 아이디를 빈배열에 저장
        var person:[NSManagedObject] = []
        
        do {
            person = try context.fetch(request) // person에 request저장
        } catch {
            print("Error!")
        }
        
	// person 배열에 id값이 저장된 것이 없을때
        if person.count == 0{
            displayErrorMessage(title: "회원이 없습니다.", message: "회원정보를 다시 확인해주세요.")
            return // 바로 리턴을 해서 종료
        }
        
	// person[0]:: 저장된 값이 1개 밖에 없으니까. 아이디는 중복 불가
        let pw = person[0].value(forKey: "password") as! String
        
        if pw == pwField.text!{
            user = person[0]
            performSegue(withIdentifier: "GoToMypage", sender: nil)
        } else {
            displayErrorMessage(title: "비밀번호 오류", message: "비밀번호를 다시 입력해주세요.")
        }
        
    }
    // GoToMypage segue가 맞다면 LoginPageViewController의 user 변수에 넘길 데이터를 저장
    override func prepare(for segue: UIStoryboardSegue, sender: Any?){
        if segue.identifier == "GoToMypage" {
            let mypageController = segue.destination as! LoginPageViewController
            mypageController.user = user // LoginPageViewController의 user변수로 값 데이터 저장
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        // keyboard
        let tap: UITapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(ViewController.dismissKeyboard))
        view.addGestureRecognizer(tap)
    }
    
    
    // keyboard
    @objc func dismissKeyboard(){
        self.view.endEditing(true)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}
```

## JoinViewController

```swift
import UIKit
import CoreData

class JoinViewController: UIViewController {
    @IBOutlet weak var id: UITextField!
    @IBOutlet weak var pw: UITextField!
    @IBOutlet weak var name: UITextField!
    @IBOutlet weak var birth: UITextField!
    
    func displyaErrorMessage(title: String, message: String) {
        let alert = UIAlertController(title: title, message: message, preferredStyle: .alert)
        let confirm = UIAlertAction(title: "확인", style: .default) { (action: UIAlertAction) -> Void in }
        alert.addAction(confirm)
        present(alert, animated: true, completion: nil)
     }
    
    @IBAction func joinSuccess(_ sender: UIButton) {
        
        // 빈 문자열 오류 표시
        if name.text! == "" || id.text! == "" || pw.text! == "" || birth.text! == "" {
            displyaErrorMessage(title: "저장을 할 수 없습니다.", message: "한칸의 빈칸도 남기면 안됩니다!")
            return
        }
        
        let appDelegate = UIApplication.shared.delegate as! AppDelegate
        
        let context = appDelegate.persistentContainer.viewContext
        
        let request = NSFetchRequest<NSManagedObject>(entityName: "User")
        
        // 아이디가 1개 이상일때 중복메세지
        request.predicate = NSPredicate(format: "id == %@", id.text!)
        
        var person: [NSManagedObject] = []
        
        do {
            person = try context.fetch(request)

            // person엔 1개 아이디만 등록된다. 중복불가
            if person.count != 0 {
                displyaErrorMessage(title: "저장을 할 수 없습니다.", message: "아이디가 중복됩니다.")
                return
            }
            
        } catch {
            print("ERROR!")
        }
        
        let user = NSEntityDescription.insertNewObject(forEntityName: "User", into: context)

        // User에 저장
        user.setValue(name.text!, forKey: "name")
        user.setValue(id.text!, forKey: "id")
        user.setValue(pw.text!, forKey: "password")
        user.setValue(birth.text!, forKey: "birth")
        
        if context.hasChanges{
            do {
                try context.save()
                print("Success!")
                performSegue(withIdentifier: "GotoStart", sender: nil)
            } catch {
                print("Error!")
            }
        }
        
    }
    
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
        
        let tap: UITapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(JoinViewController.dismissKeyboard))
        view.addGestureRecognizer(tap)
    }
    
    @objc func dismissKeyboard(){
        self.view.endEditing(true)
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
}
```

## LoginPageViewController

```swift
import UIKit
import CoreData

class LoginPageViewController: UIViewController {
    
    // ViewController의 user변수에서 온 값을 저장하고 있다.
    var user: NSManagedObject? = nil
    
    @IBOutlet weak var id: UILabel!
    @IBOutlet weak var birth: UILabel!
    @IBOutlet weak var name: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.

	// user에서는 무조건 값이 넘어온다.
        id.text = user!.value(forKey: "id") as? String
        birth.text = user!.value(forKey: "birth") as? String
        name.text = user!.value(forKey: "name") as? String
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

}
```
