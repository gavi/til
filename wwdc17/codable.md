# Codable
Codable protocol in Foundation enables JSON encoding and decoding.

## Encoding JSON 

The below example shows 2 floats plus a date object. Not the date encoding strategy

```swift
import UIKit

struct Point:Codable{
    var x:Float
    var y:Float
    var moment:Date
}

let p=Point(x:10,y:10,moment:Date())
let x=JSONEncoder()
x.dateEncodingStrategy = .iso8601
let jsonData=try? x.encode(p)
if let data=jsonData{
    print(String(data:data , encoding: .utf8)!)
}
```

## Decoding JSON

Decoding works the same way as encoding

```swift
let inputData = """
        {"x":10,"y":10,"moment":"2017-06-08T21:36:37Z"}
        """.data(using: .utf8)!
let decoder = JSONDecoder()
decoder.dateDecodingStrategy = .iso8601
let obj=try? decoder.decode(Point.self, from: inputData)
if let point=obj{
    print(point.x)
}

```

## Renaming the json keys 


## Error handling


## Nested Structures


## Ignoring stuff that you do not need in your model

