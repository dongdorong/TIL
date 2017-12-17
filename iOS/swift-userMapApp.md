# 사용자 위치 정보 Application
- 사용자의 위치값이 바뀔때마다 mark가 따라다닌다.
- Tab으로 지도와, 위치 숫자 정보를 보여준다.
- startUpdatingLocation 함수가 불려질때마다 didUpdateLocations가 없데이트 된다.
- mark가 1개 보여지기 위해서는 didUpdateLocations에 mapView.removeAnnotations(mapView.annotations)를 써줘야한다.(누적이 안되도록)


<img src="/img/mapView_01.png" width="300">
<img src="/img/mapView_02.png" width="300">

### FirstViewController
 ```swift
import UIKit
import MapKit

class FirstViewController: UIViewController, CLLocationManagerDelegate {
    
    @IBOutlet weak var mapView: MKMapView!
    
    let locManager = CLLocationManager()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        // 기본설정
        locManager.delegate = self
        locManager.requestAlwaysAuthorization()
        locManager.desiredAccuracy = kCLLocationAccuracyBest // 베터리 소모가 잘됨
        locManager.startUpdatingLocation() // 이 함수가 불려질때마다 didUpdateLocations가 없데이트
    }
    
    func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        // 위치 정보가 바뀔때마다 누적되는 mark 삭제
        mapView.removeAnnotations(mapView.annotations)
        
        let location = locations[0]
        
        // 화면에서 보이는 지도의 폭의 값:: 값이 작아질 수록 지도 확대
        let latDel: CLLocationDegrees = 0.005
        let longDel: CLLocationDegrees = 0.005
        
        // 실제 폭 정의
        let mapSpan = MKCoordinateSpan(latitudeDelta: latDel, longitudeDelta: longDel)
        
        // 사용자의 위치 중심으로 center 설정
        let region = MKCoordinateRegion(center: location.coordinate, span: mapSpan)
        
        mapView.setRegion(region, animated: true)
        
        let annotation = MKPointAnnotation()
        
        // mark 좌표 설정
        annotation.coordinate = location.coordinate
        
        // mark title 설정
        annotation.title = "User Location"
        annotation.subtitle = "Current Location Here"
        
        // 지도에 mark 띄우기
        mapView.addAnnotation(annotation)
        
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

}
 ```

### SecondViewController
```swift
import UIKit
import MapKit

class SecondViewController: UIViewController, CLLocationManagerDelegate {
    @IBOutlet weak var latitude: UILabel!
    @IBOutlet weak var longtude: UILabel!
    @IBOutlet weak var speedText: UILabel!
    @IBOutlet weak var altitudeText: UILabel!
    
    let locManager = CLLocationManager()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        locManager.delegate = self
        locManager.requestAlwaysAuthorization()
        locManager.desiredAccuracy = kCLLocationAccuracyBest // 베터리 소모가 잘됨
        locManager.startUpdatingLocation() // 이 함수가 불려질때마다 didUpdateLocations가 없데이트
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        let location = locations[0]
        
        latitude.text! = String(location.coordinate.latitude)
        longtude.text! = String(location.coordinate.longitude)
        speedText.text! = String(location.speed)
        altitudeText.text! = String(location.altitude)
        
    }


}
```

