# Draw a background Color for NSView in Swift

For quick tests you might need to draw a background color for a NSView. Here is the sample code

```swift
class NSColorView:NSView{
    override func drawRect(dirtyRect: NSRect) {
        NSColor.redColor().setFill()
        NSRectFill(dirtyRect)
        super.drawRect(dirtyRect)
    }
}
```

Change the color as necessary