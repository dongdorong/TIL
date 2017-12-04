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
