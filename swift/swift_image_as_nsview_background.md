# NSImage as a background of NSView in Swift

I needed to draw an NSImage as a background for an NSView for printing purposes. Here is my NSView that takes an NSImage as a parameter and draws it as part of the background

```swift
class MyNSView:NSView{
    
    var img:NSImage?
    
    override init(frame frameRect: NSRect) {
        super.init(frame: frameRect)
    }
    
    required init?(coder: NSCoder) {
        super.init(coder: coder)
    }
    
    init(frame frameRect: NSRect,img:NSImage){
        self.img=img
        super.init(frame: frameRect)
        
    }
    
    override func drawRect(dirtyRect: NSRect) {
        NSColor.init(patternImage:self.img!).setFill()
        NSRectFill(dirtyRect)
        super.drawRect(dirtyRect)
    }
}
```

