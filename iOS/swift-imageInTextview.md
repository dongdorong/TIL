# Image In UITextView
- UItextView안에 이미지를 넣고 싶을때!
- viewDidLoade or viewWillAppear에 사용하고 있다.
- contentMode 맞추는 작업 해야함.

```swift
var attributedString :NSMutableAttributedString!
attributedString = NSMutableAttributedString(attributedString:contentsView.attributedText)
let textAttachment = NSTextAttachment()
textAttachment.image = UIImage(named: "02.png")

let oldWidth = textAttachment.image!.size.width;

let scaleFactor = oldWidth / (contentsView.frame.size.width + 30)
textAttachment.image = UIImage(cgImage: textAttachment.image!.cgImage!, scale: scaleFactor, orientation: .up)
let attrStringWithImage = NSAttributedString(attachment: textAttachment)
attributedString.append(attrStringWithImage)
contentsView.attributedText = attributedString

```