# 사용자 위치 정보 Application

 ```swift
 //
//  FirstViewController.swift
//  UserMap
//
//  Created by donghwa on 2017. 12. 11..
//  Copyright © 2017년 donghwa. All rights reserved.
//

import UIKit
import CoreLocation
import MapKit

class FirstViewController: UIViewController, CLLocationManagerDelegate, MKMapViewDelegate {
    
    @IBOutlet weak var mapView: MKMapView!
    
    let locManager = CLLocationManager()
    
    let latitude: CLLocationDegrees = 37.48
    let longitude: CLLocationDegrees = 127.0
    let latDel: CLLocationDegrees = 0.05
    let longDel: CLLocationDegrees = 0.05
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        let center = CLLocationCoordinate2D(latitude: latitude, longitude: longitude)
        
        let mapSpan = MKCoordinateSpan(latitudeDelta: latDel, longitudeDelta: longDel)
        
        let region = MKCoordinateRegion(center: center, span: mapSpan)
        
        mapView.setRegion(region, animated: true)
        
        locManager.delegate = self
        locManager.requestAlwaysAuthorization()
        locManager.desiredAccuracy = kCLLocationAccuracyBest
        locManager.startUpdatingLocation()
    
    }
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        let location = locations[0]
        print(location.altitude)
        print(location.coordinate)
        print(location.speed)
        
        let annotation = MKPointAnnotation()
        annotation.title = "test"
        annotation.subtitle = "test inside"
        annotation.coordinate = location.coordinate
        mapView.addAnnotation(annotation)
    }

}
 ```
