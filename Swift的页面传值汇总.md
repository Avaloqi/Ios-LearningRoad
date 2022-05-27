# Swift的页面传值总结 
注：页面实现使用StoryBoard

## 基础页面搭建
每个页面一个Button、一个textField. 
ViewContoler01. 


    import UIKit

    class ViewController01: UIViewController {
    
        @IBOutlet var textField01: UITextField?

        override func viewDidLoad() {
            super.viewDidLoad()
        }
    
        @IBAction func make01(_ sender: Any) {
            let vc = ViewController02()
            if textField01?.text != nil {
                vc.textField02?.text = textField01?.text
            } else {
                vc.textField02?.text = "I am coming"
                vc.str = "I am coming"
            }
        
            navigationController?.pushViewController(vc, animated: true)
        }
    }



## 属性传值

