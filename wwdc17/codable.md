# Codable
Codable protocol in Foundation enables JSON encoding and decoding.

## Encoding JSON 

The below example shows 2 floats plus a date object. Note the date encoding strategy

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

A little bit more complicated example where we are renaming the keys from `formatted_address` to `formattedAddress`. 

```swift
let geoResult="""
{
  "results": [
    {
      "formatted_address": "1600 Amphitheatre Parkway, Mountain View, CA 94043, USA",
      "geometry": {
        "location": {
          "lat": 37.4224764,
          "lng": -122.0842499
        }
      }
    }
  ],
  "status": "OK"
}
""".data(using: .utf8)!

struct GeocodingService:Codable{
    var status:String
    var results:[GeocodingResult]
}

struct GeocodingResult:Codable{
    struct Geometry:Codable{
        struct Location:Codable{
            let lat:Float
            let lng:Float
        }
        let location:Location
    }
    let formattedAddress:String
    let geometry:Geometry
    private enum CodingKeys: String, CodingKey {
        case formattedAddress = "formatted_address"
        case geometry
    }
}

let decoder=JSONDecoder()
do{
    let obj=try decoder.decode(GeocodingService.self, from: geoResult)
    for result in obj.results{
        print("(\(result.geometry.location.lat),\(result.geometry.location.lng))")
    }
}catch{
    print("\(error)")
}
```

## Error handling

Use the catch block to print out errors

```swift
do{

}catch{
    print("\(error)")
}
```

