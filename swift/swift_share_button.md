# Sharing functionality in Swift Mac App

To create basic sharing functionality with some text and an image here is a quick snippet

```swift
 	@IBAction func showSharingPicker(sender: AnyObject) {
    	//Any function that generates an Image
        let img: NSImage = Util.getScreenshot(viewController!.view)
        var arr: [AnyObject] = [AnyObject]()
        arr.append(img)
        arr.append("\(fileName!)")
        arr.append("@objectgraph")
        arr.append("https://itunes.apple.com/us/app/id1096825035")
        let sharingServicePicker: NSSharingServicePicker = NSSharingServicePicker(items: arr)
        sharingServicePicker.showRelativeToRect(sender.bounds, ofView: sender as! NSView, preferredEdge: NSRectEdge.MinY)
    }

```

Simply create an array, add the `NSImage` and each text blob seperately. The output is similar to below

[[http://webapps.objectgraph.com/til/img/sharing.png]]