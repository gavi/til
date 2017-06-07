# MLKit

MLKit allows you to quickly use external created models (For example with scikit-learn) and use them in Xcode projects.

Download Models from https://developer.apple.com/machine-learning/

One of the example `.mlmodel` file that was provided by Apple was the `Inceptionv3.mlmodel`. Importing this to Xcode created a stub class file for interacting with it. This class takes a `CVPixelBuffer` and classifies what the model sees in the image.

One utility function is needed for converting UIImage to CVPixelBuffer is shown below 

```swift
func getCVPixelBuffer(_ image: CGImage) -> CVPixelBuffer? {
        let imageWidth = Int(image.width)
        let imageHeight = Int(image.height)
        
        let attributes : [NSObject:AnyObject] = [
            kCVPixelBufferCGImageCompatibilityKey : true as AnyObject,
            kCVPixelBufferCGBitmapContextCompatibilityKey : true as AnyObject
        ]
        
        var pxbuffer: CVPixelBuffer? = nil
        CVPixelBufferCreate(kCFAllocatorDefault,
                            imageWidth,
                            imageHeight,
                            kCVPixelFormatType_32ARGB,
                            attributes as CFDictionary?,
                            &pxbuffer)
        
        if let _pxbuffer = pxbuffer {
            let flags = CVPixelBufferLockFlags(rawValue: 0)
            CVPixelBufferLockBaseAddress(_pxbuffer, flags)
            let pxdata = CVPixelBufferGetBaseAddress(_pxbuffer)
            
            let rgbColorSpace = CGColorSpaceCreateDeviceRGB();
            let context = CGContext(data: pxdata,
                                    width: imageWidth,
                                    height: imageHeight,
                                    bitsPerComponent: 8,
                                    bytesPerRow: CVPixelBufferGetBytesPerRow(_pxbuffer),
                                    space: rgbColorSpace,
                                    bitmapInfo: CGImageAlphaInfo.premultipliedFirst.rawValue)
            
            if let _context = context {
                _context.draw(image, in: CGRect.init(x: 0, y: 0, width: imageWidth, height: imageHeight))
            }
            else {
                CVPixelBufferUnlockBaseAddress(_pxbuffer, flags);
                return nil
            }
            
            CVPixelBufferUnlockBaseAddress(_pxbuffer, flags);
            return _pxbuffer;
        }
        
        return nil
    }
```

To Call the Inception model the following sample code is used.

```swift
let output=try? inception.prediction(image: getCVPixelBuffer(pickedImage.cgImage!)!)
mLabel.text=(output?.classLabel)
```


## SciKit-Learn  to .mlmodel

Here is my simple python sklearn 

```python
from sklearn import datasets
from sklearn.svm import SVC
from sklearn.externals import joblib
import pickle
iris = datasets.load_iris()
clf = SVC()
clf.fit(iris.data, iris.target_names[iris.target])  
print(list(clf.predict(iris.data[:3])))
#joblib.dump(clf, 'iris.pkl') 
filename = 'iris.pkl'
pickle.dump(clf, open(filename, 'wb'))
```

Then I downloaded `coremltools` using `pip` on conda and tried to convert the pickle file, but no luck. But the basic idea remains, you can convert your exported models to `.mlmodel` format using the `coremltools` open source project from Apple.



