# IOS Code Snippets
## To get Class Name 
To do this write a extension for NSObject class
```swift

extension NSObject {
    class var name: String {
        return String(describing: self)
    }
    
    var name: String {
        return String(describing: self)
    }
}

```
To print class name(let say of class ImageViewCell) we have to write:
```swift

print(ImageViewCell.name)

```

## Manage The Keyboard

```swift
//In viewdidload(){
	//..... add bellow 2 lines in ViewDidLoad() Function 
	NotificationCenter.default.addObserver( self, selector: #selector(keyboardWillShow), name: NSNotification.Name.UIKeyboardWillShow, object: nil)
         NotificationCenter.default.addObserver( self, selector: #selector(keyboardWillHide), name: NSNotification.Name.UIKeyboardWillHide, object: nil)
//}


@objc func keyboardWillShow( notification :NSNotification ) {
        if let keyboardFrame = (notification.userInfo?[ UIKeyboardFrameEndUserInfoKey ] as? NSValue)?.cgRectValue {
            var insets: UIEdgeInsets
            insets =  UIEdgeInsetsMake( 0, 0, keyboardFrame.height, 0 )
            tableView.contentInset = insets
            tableView.scrollIndicatorInsets = insets
        }
    }
@objc func keyboardWillHide( notification :NSNotification ) {
        var insets: UIEdgeInsets
        insets = UIEdgeInsetsMake( 0, 0, 0, 0 )
        tableView.contentInset = insets
        tableView.scrollIndicatorInsets = insets
        
    }

```
## Show only bottom border of textField
To this simply add below extension for UITextField class.

```swift

extension UITextField {
    func setBottomBorder() {
        self.borderStyle = .none
        self.layer.backgroundColor = UIColor.white.cgColor
        self.layer.masksToBounds = false
        self.layer.shadowColor = UIColor.lightGray.cgColor
        self.layer.shadowOffset = CGSize(width: 0.0, height: 1.0)
        self.layer.shadowOpacity = 0.4
        self.layer.shadowRadius = 0.0
    }
}

```
for set only bottom border we have to call setBottomBorder() function on textField Object(let say numberTextField: UITextField).
```swift

numberTextField.setBottomBorder()

```

## Only numbers are allowed to enter in Textfeld

```swift

func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
	let allowedCharacters = CharacterSet(charactersIn: "1234567890")
        let characterSet = CharacterSet(charactersIn: string)
        if allowedCharacters.isSuperset(of: characterSet) && (textField.text!.count < 10){
            numbers[textField.tag].number = textField.text! + string
            return true
        }
        return false
}

```

## Placeholde in UITextView
```swift

import UIKit

class ViewController: UIViewController {

	let placeholder = "Placeholder"
	
	@IBOutlet weak var textView: UITextView!
	
	override func viewDidLoad() {
		super.viewDidLoad()
		
		textView.delegate = self
		textView.text = placeholder
		textView.textColor = UIColor.lightGray
		textView.selectedTextRange = textView.textRange(from: textView.beginningOfDocument, to: textView.beginningOfDocument)
	}

}

extension ViewController: UITextViewDelegate {
	
	func textView(_ textView: UITextView, shouldChangeTextIn range: NSRange, replacementText text: String) -> Bool {
		
		let currentText: NSString = textView.text as NSString
		let updatedText = currentText.replacingCharacters(in: range, with:text)
		
		if updatedText.isEmpty {
			textView.text = placeholder
			textView.textColor = UIColor.lightGray
			textView.selectedTextRange = textView.textRange(from: textView.beginningOfDocument, to: textView.beginningOfDocument)
			return false
		}
		else if textView.textColor == UIColor.lightGray && !text.isEmpty {
			textView.text = nil
			textView.textColor = UIColor.black
		}
		
		return true
	}
	
	func textViewDidChangeSelection(_ textView: UITextView) {
		if self.view.window != nil {
			if textView.textColor == UIColor.lightGray {
				textView.selectedTextRange = textView.textRange(from: textView.beginningOfDocument, to: textView.beginningOfDocument)
			}
		}
	}
	

```
## Apply ImagePicker

```swift

//MARK: - Apply Image picker when button is click whose Action function is pickAnImage(_ Sender:) 
 @IBAction func pickAnImage(_ sender: UIBarButtonItem) {
        let imagePickerController = UIImagePickerController()
        imagePickerController.sourceType = .photoLibrary  //image is selected from gallary
	//imagePickerController.sourceType = .camera      //image is selected from camera
        imagePickerController.allowsEditing = true
        imagePickerController.delegate = self
        self.present(imagePickerController, animated: true, completion: nil)
    }
   
```
#### NOTE
When image is selected from camera we have to check is cmera Hardware is present or not(in case when we test app in simulator where no camera Harware is present) otherewise app can crash if there is no camera hardware.
we can check it by adding a simple line in our code:
```swift

let isCamerePresent: Bool = UIImagePickerController.isSourceTypeAvailable(.camera)

```
This return **True** if camera hardware is present otherwise false.

#### Get selected image from Image Picker Controller 
To get The selected Image we have to use UIImagePickerController delegate method imagePickerController(_ picker: , didFinishPickingMediaWithInfo info:).
```swift

func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [String : Any]) {
        if let image:UIImage = (info[UIImagePickerControllerEditedImage] as? UIImage){
            self.imageView.image = image	//set image for imageView
            self.dismiss(animated: true, completion: nil)
        }
}

```
