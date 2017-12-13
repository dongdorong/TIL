# 지도 (mapView)

<img src="/img/mapView.png" width="300">

- mapView 사용하기
- CLLocationDegrees:: 위도 경도, 얼마나 zoom할지 결정
- CLLocationCoordinate2D:: 실제 위치를 나타내 주는 것
- MKCoordinateSpan:: mapView가 알아듣게 명령해주는 것
- MKCoordinateRegion:: 보이는 지역 설정

```swift
import UIKit
import MapKit

class ViewController: UIViewController, MKMapViewDelegate {

    @IBOutlet weak var mapView: MKMapView!

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.

        // 위도
        let latitude: CLLocationDegrees = 37.57

        // 경도
        let longitude: CLLocationDegrees = 127.0

        // 실제 위치를 나타내 주는 것
        let center = CLLocationCoordinate2D(latitude: latitude, longitude: longitude)

        // zoom
        let latDel: CLLocationDegrees = 0.05
        let longDel: CLLocationDegrees = 0.05

        // zoom을 mapView가 알아 듣게 명령
        let mapSpan = MKCoordinateSpan(latitudeDelta: latDel, longitudeDelta: longDel)

        // 보여지는 지역 설정
        let region = MKCoordinateRegion(center: center, span: mapSpan)

        // mapView의 Region 설정
        mapView.setRegion(region, animated: true)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}
```

# mark 표시&삭제
- UILongPressGestureRecognizer:: 오래 눌렀을때 감지
- longPressRecognizer.minimumPressDuration:: 누르는 초 설정
- gestureRecognizer.state == .began:: 감지가 시작될때만 체크
- MKPointAnnotation:: 지도에 mark 추가


```swift
class ViewController: UIViewController, MKMapViewDelegate {
    
    @IBOutlet weak var mapView: MKMapView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        // 위도
        let latitude: CLLocationDegrees = 37.57
        
        // 경도
        let longitude: CLLocationDegrees = 127.0
        
        // 실제 위치를 나타내 주는 것
        let center = CLLocationCoordinate2D(latitude: latitude, longitude: longitude)
        
        // zoom
        let latDel: CLLocationDegrees = 0.05
        let longDel: CLLocationDegrees = 0.05
        
        // zoom을 mapView가 알아 듣게 명령
        let mapSpan = MKCoordinateSpan(latitudeDelta: latDel, longitudeDelta: longDel)
        
        // 보여지는 지역 설정
        let region = MKCoordinateRegion(center: center, span: mapSpan)
        
        // mapView의 Region 설정
        mapView.setRegion(region, animated: true)
        
        // touch 감지
        let tapRecognizer = UITapGestureRecognizer(target: self, action: #selector(ViewController.tap(gestureRecognizer:)))
        mapView.addGestureRecognizer(tapRecognizer)
        
        // long touch 감지
        let longPressRecognizer = UILongPressGestureRecognizer(target: self, action: #selector(ViewController.longPress(gestureRecognizer:)))
        
        // long touch 초 설정
        longPressRecognizer.minimumPressDuration = 1.0
        
        mapView.addGestureRecognizer(longPressRecognizer)
    }
    
    @objc func longPress(gestureRecognizer: UILongPressGestureRecognizer){
        // 감지가 시작될때만 메세지 출력:: 여러번 출력되는 것을 방지
        if gestureRecognizer.state == .began{
            
            // mark 추가 방법
            
            // touch 감지
            let touch = gestureRecognizer.location(in: self.mapView)
            
            // 화면에서 일어난 touch를 mapView의 좌표로 convert 명령
            let coordinate = mapView.convert(touch, toCoordinateFrom: self.mapView)
            
            // mark 추가
            let annotation = MKPointAnnotation()
            
	    // touch한 위치에 mark하라는 명령
            annotation.coordinate = coordinate
            
            annotation.title = "new mark"
            annotation.subtitle = "please add new mark"
            
            mapView.addAnnotation(annotation)
        }
    }
    
    @objc func tap(gestureRecognizer: UIGestureRecognizer){
        // 짧게 터치했을 경우 mark 삭제
        mapView.removeAnnotations(mapView.annotations)
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}
```
# 사용자의 위치 정보 받기
- Build Phases > Link Binary With Libraries > CoreLocation framwork 추가
- info.plist::
	+ Privacy - Location Always and When In Use Usage Description:: 위치 정보를 왜 알아야 하는지
	+ Privacy - Location Always Usage Description:: 앱이 실행하지 않아도 위치 정보를 왜 알아야 하는지
	+ 각각 알맞은 이유를 추가해준다.
- import CoreLocation:: User의 위치 정보를 받아올수 있게 도와주는 framwork
- CLLocationManagerDelegate:: 위치 정보의 변화가 있을때 ViewController에서 그 변화를 감지하고 처리 하겠다는 것
- kCLLocationAccuracyBest:: 최고의 정확도로 위치 정보를 받아온다 `배터리 소모가 크다`
- didUpdateLocations:: 위치 정보가 바뀔때 마다
- coordinate:: 좌표
- speed:: 속도
- altitude:: 고도

```swift
import UIKit
import CoreLocation

class ViewController: UIViewController, CLLocationManagerDelegate {

    // 위치 정보를 받아오는 도구
    let locManager = CLLocationManager()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        // locManager의 delegate를 ViewController로 설정
        locManager.delegate = self
        
        // 위치 정보를 받을때에는 사용자에게 허락을 항상 받는다.
        locManager.requestAlwaysAuthorization()
        
        // 정확도 설정:: Best는 배터리 소모가 크다
         locManager.desiredAccuracy = kCLLocationAccuracyBest
        
        // 위치 정보 받기 시작
        locManager.startUpdatingLocation()
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    // 위치 정보가 변경될때마다 호출할 함수
    func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        let location = locations[0]
        
        print(location.coordinate) // 좌표정보
        print(location.speed) // 속도
        print(location.altitude) // 고도
    }

}
```
