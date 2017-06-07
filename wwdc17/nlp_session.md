# NLP using NSLinguisticTagger 

Use `NSLinguisticTagger` to extract sentences, domainant language, lemma, lexical class etc.

## Basic Setup

```swift
var str = "This is some text that i do not know how it runs?"

let tagger=NSLinguisticTagger(tagSchemes: [.lemma, .language, .lexicalClass], options:0 )
tagger.string=str
let range=NSRange(location:0,length:str.utf16.count)
print(tagger.dominantLanguage!)
```

## Now interate through the tokens and tags


```swift
tagger.enumerateTags(in: range, unit: .word, scheme:.lemma, options: [.omitPunctuation, .omitWhitespace]) { tag, tokenRange, _ in
    let token = (str as NSString).substring(with: tokenRange)
    // Each word of the text is inserted into the result set (in lowercase form).
    print("word:\(token.lowercased())")
    
    if let lemma = tag?.rawValue {
        // If there is a lemma, it is also inserted into the result set (in lowercase form).
        print("lemma:\(lemma.lowercased())")
    }   
}
```