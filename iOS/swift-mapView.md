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
