# CoreData
- 비교적 복잡하고 많은 양의 정보를 저장할 때 사용
1. 어떤 정보를 저장할 것인지 Entity 생성
2. 객체 저장

### Entity 생성
#### PersonSave.xcdatamodeld
- new Entity 
    + "name" Attribute 생성
    + "national" Attribute 생성

#### viewController

#### Data 저장
- delegate:: 
    + 화면 터치 할때 사용
    + App의 상태가 바뀔때 사용
- persistentContainer:: App이 종료되어도 데이터를 저장하는 곳
- NSEntityDescription:: CoreData에서의 Entity 정보를 담고 있는 class
- insertNewObject:: insertNewObject를 통해서 데이터 담는 곳인 context에 "Person" 객체를 생성하여 담아주는 메서드
- 메모리 부족 등의 에러가 날 가능성이 있기에 저장을 할땐 do catch를 이용한다.
    
```swift
// coredata를 import 해준다.
import CoreData

override func viewDidLoad() {
        super.viewDidLoad()

    let appDelegate = UIApplication.shared.delegate as! AppDelegate
        
    let context = appDelegate.persistentContainer.viewContext
    
    // 저장할 때
    let person = NSEntityDescription.insertNewObject(forEntityName: "Person", into: context)

    // Attribute 생성
    person.setValue("Anggo", forKey: "name")
    person.setValue("Korea", forKey: "national")

    func saveContext () {

        if context.hasChanges {
            do {
                try context.save() // 영구적으로 저장하라는 명령
                print("Person Save!")
            } catch {
                let nserror = error as NSError
                fatalError("Unresolved error \(nserror), \(nserror.userInfo)")
            }
        }
    }

    saveContext() // 실행
    
}
```

### 저장한 Data 가져오기
- NSFetchRequest:: Entity에 저장된 값을 받아 올 때 사용. NSFetchRequest를 통해서 "Person" Entity를 <NSManagedObject> 타입으로 리턴해서 값을 받아온다.
- NSPredicate:: 원하는 값을 가져올 때 사용. 데이터를 어떻게 Fetch할 것인지 패턴을 정의할 때 사용한다. ex) name의 "Dona" 값만 받아온다.
- NSManagedObject 타입의 데이터를 받아 올 빈 배열 생성
- DispatchQueue.global().async:: 많은 양의 데이터를 가져올때 화면이 멈춰보일 수 있으니 비동기로 처리
    + DispatchQueue.main.async:: 비동기로 UI update할때 사용
- NSPredicate로 불러온 데이터를 삭제한 후 do catch문으로 한번 더 저장해준다.
 
```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view, typically from a nib.
    
    let appDelegate = UIApplication.shared.delegate as! AppDelegate
    
    let context = appDelegate.persistentContainer.viewContext
    
    // 저장된 값 가져오기
    let  request = NSFetchRequest<NSManagedObject>(entityName: "Person")
    
    // 저장할 때
    let person = NSEntityDescription.insertNewObject(forEntityName: "Person", into: context)

    person.setValue("Anggo", forKey: "name")
    person.setValue("Korea", forKey: "national")

    func saveContext () {

        if context.hasChanges {
            do {
                try context.save()
                print("Person Saved!")
            } catch {
                let nserror = error as NSError
                fatalError("Unresolved error \(nserror), \(nserror.userInfo)")
            }
        }
    }

    saveContext()
    
    
    // 원하는 값만 받아오고 싶을 때
    request.predicate = NSPredicate(format: "name == %@", "Anggo")
    
    // 리턴타입이 NSManagedObject니까 같은 타입의 빈배열을 만들어 준다.
    var people: [NSManagedObject] = []
    
    // 비동기식 처리
    DispatchQueue.global().async {
        func receiveContext() {
            do{
                try people = context.fetch(request)
                
                for i in people{
                    print("name: \(i.value(forKey: "name")!) \nnataional: \(i.value(forKey: "national")!) \n\n")
                    
                    // 형변환해서 받을 때
                    var named =  i.value(forKey: "name") as! String
                    
                    // NSPredicate로 지정한 데이터를 삭제 할 때
                    context.delete(i)
                    
                    // 삭제한 데이터를 저장해준다.
                    if context.hasChanges{
                        do {
                            try context.save()
                        } catch {
                            let nserror = error as NSError
                            fatalError("Unresolved error \(nserror), \(nserror.userInfo)")
                        }
                    }
                    
                }
            } catch {
                let nserror = error as NSError
                fatalError("Unresolved error \(nserror), \(nserror.userInfo)")
            }
        }
        receiveContext()
        
        // UI 업데이트를 하고 싶을 때
        DispatchQueue.main.async {
            
        }
    }
    
}
``` 