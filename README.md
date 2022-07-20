# Adding-leading-constraint-programmaticallimport UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        //addViewStandard()
        addConstraintsView()
    }

    func addConstraintsView() {
        let someView = UIView(frame: CGRect.zero)
        someView.backgroundColor = UIColor.blue
        // I want to mimic a frame set of CGRect(x: 20, y: 50, width: 50, height: 50)
        let widthConstraint = NSLayoutConstraint(item: someView, attribute: .width, relatedBy: .equal, toItem: nil, attribute: .notAnAttribute, multiplier: 1.0, constant: 50)
        let heightConstraint = NSLayoutConstraint(item: someView, attribute: .height, relatedBy: .equal, toItem: nil, attribute: .notAnAttribute, multiplier: 1.0, constant: 50)
        let leadingConstraint = NSLayoutConstraint(item: someView, attribute: .leading, relatedBy: .equal, toItem: view, attribute: .leading, multiplier: 1.0, constant: 20)
        someView.translatesAutoresizingMaskIntoConstraints = false
        someView.addConstraints([widthConstraint, heightConstraint, leadingConstraint])
        view.addSubview(someView)
    }
}
Now when I run the app it crashes because of the leading constraint. The error message is "Impossible to set up layout with view hierarchy unprepared for constraint". What am I doing wrong here? Should I be adding the constraints to the object (the blue box on this case) or adding them to its superview?

EDIT:

After code changes I have:

func addConstraintsView() {
        let someView = UIView(frame: CGRect.zero)
        someView.backgroundColor = UIColor.blue
        view.addSubview(someView)
        someView.translatesAutoresizingMaskIntoConstraints = false
        let widthConstraint = NSLayoutConstraint(item: someView, attribute: .width, relatedBy: .equal, toItem: nil, attribute: .notAnAttribute, multiplier: 1.0, constant: 50)
        let heightConstraint = NSLayoutConstraint(item: someView, attribute: .height, relatedBy: .equal, toItem: nil, attribute: .notAnAttribute, multiplier: 1.0, constant: 50)
        let leadingConstraint = NSLayoutConstraint(item: someView, attribute: .leading, relatedBy: .equal, toItem: view, attribute: .left, multiplier: 1.0, constant: 20)
        someView.addConstraints([widthConstraint, heightConstraint])
        view.addConstraints([leadingConstraint])
    }
ANSWER

First of all,


copy icon

download icon

view.addSubview(someView)
someView.translatesAutoresizingMaskIntoConstraints = false
should come before the constraints phase; you have to apply the constraints AFTER someView is added to its superview.

Also, if you are targeting iOS 9, I'd advise you to use layout anchors like


copy icon

download icon

let widthConstraint = someView.widthAnchor.constraint(equalToConstant: 50.0)
let heightConstraint = someView.heightAnchor.constraint(equalToConstant: 50.0)
let leadingConstraint = someView.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 20.0)
NSLayoutConstraint.activate([widthConstraint, heightConstraint, leadingConstraint])
This way you don't have to worry about which view to apply the constraints to.

Finally (and to clear up your doubt), if you can't use layout anchors, you should add the leading constraint to the superview, not the view.
