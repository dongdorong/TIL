# AudioPlayer
- 음성파일을 사용하려면 AVFoundation를 import (AV:: AudioVisual)
- Bundle.main.path:: Bundle은 프로젝트 내에 있는 파일 저장소. 파일 경로를 설정해준다.
- sender.value:: Slider의 value

```swift
import UIKit
import AVFoundation

class ViewController: UIViewController {

    // 음성파일을 재생시켜주는 도구(Player)
    var musicPlayer = AVAudioPlayer()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        // 경로 설정
        let audioPath = Bundle.main.path(forResource: "LostandFound", ofType: "mp3")
        
        do {
            try musicPlayer = AVAudioPlayer(contentsOf: URL(fileURLWithPath: audioPath!))
        } catch  {
            print("ERROR!!")
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    @IBAction func start(_ sender: UIButton) {
        // 재생
        musicPlayer.play()
    }
    
    @IBAction func stop(_ sender: UIButton) {
        // 정지
        musicPlayer.stop()
    }
    
    @IBAction func volume(_ sender: UISlider) {
	// 볼륨 조절
        musicPlayer.volume = sender.value
    }
    
    
}
```