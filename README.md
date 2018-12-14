# IOS Code Snippets

### Manage The Keyboard

```swift
//In viewdidload(){
	//..... add bellow 2 lines in ViewDidLoad() Function 
	NotificationCenter.default.addObserver( self, selector: #selector(keyboardWillShow), name: NSNotification.Name.UIKeyboardWillShow, object: nil)
         NotificationCenter.default.addObserver( self, selector: #selector(keyboardWillHide), name: NSNotification.Name.UIKeyboardWillHide, object: nil)
//}


@objc func keyboardWillShow( notification :NSNotification ) {
        if let newFrame = (notification.userInfo?[ UIKeyboardFrameEndUserInfoKey ] as? NSValue)?.cgRectValue {
            var insets: UIEdgeInsets
            insets =  UIEdgeInsetsMake( 0, 0, newFrame.height, 0 )
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
