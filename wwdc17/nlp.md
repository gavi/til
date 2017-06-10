# NLP using NSLinguisticTagger 

Use `NSLinguisticTagger` to extract sentences, domainant language, lemma, lexical class etc.

## Basic Setup

```swift
import Foundation

var str = """
          This is some text that needs to be processed. I do not know how fast this runs?
          日本,
          Лорем ипсум долор сит амет, перпетуа урбанитас ин про, проприае цонсететур ид сит
         """

let tagger=NSLinguisticTagger(tagSchemes: [.lemma, .language, .lexicalClass], options:0 )
tagger.string=str
let range=NSRange(location:0,length:str.utf16.count)
print(tagger.dominantLanguage!)
```

## Now interate through the tokens and tags


```swift
func enumerate(scheme:NSLinguisticTagScheme){
    tagger.enumerateTags(in: range, unit: .word, scheme:scheme, options: [.omitPunctuation, .omitWhitespace]) {
        tag, tokenRange, _ in
        let token = (str as NSString).substring(with: tokenRange)
        print("word:\(token.lowercased())")
        if let tagVal = tag?.rawValue {
            print("\(scheme.rawValue):\(tagVal.lowercased())")
        }
    }
}

enumerate(scheme: .lexicalClass)
enumerate(scheme: .lemma)
enumerate(scheme: .language)
```