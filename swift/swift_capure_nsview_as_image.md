# Capture NSView as an Image in Swift

For printing and sharing, it is useful to take capture the current rendered `NSView` as an `NSImage` object. Here is a quick function for doing just that.

```swift
	static func getScreenshot(targetView:NSView) -> NSImage {
        let rect = targetView.convertRect(targetView.frame, toView: nil)
        let rep = targetView.bitmapImageRepForCachingDisplayInRect(rect)!
        targetView.cacheDisplayInRect(rect, toBitmapImageRep: rep)
        let image: NSImage = NSImage()
        image.addRepresentation(rep)
        return image
    }


```

