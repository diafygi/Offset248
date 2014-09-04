Offset248
=========

Offset248 is a technique for encoding binary to text that can be easily copied
and pasted. Rather than encoding bytes to another base, Offset248 keeps the data
in base 256 and simply renders the each byte into a unicode character.

###Method

The character used for each byte is a standard unicode character offset by 248.
So if you have a byte value of 0, you select the unicode character 248 ("ø").

For example, the [Wikidata favicon](https://upload.wikimedia.org/wikipedia/commons/e/e8/Wikidata-favicon.png)
 (101 bytes) encodes to:

```
ƁňņĿąĂĒĂøøøąŁŀļŊøøøĈøøøĈĀþøøøėǫǷřøøøĤŁļĹŌŰǒśŘĐĖŘĞûƻŷŜĄĊīƖƑǮėżƹĂǊŞǶĿƹǠǪƛþƄĒĸčûžĮøøğźǟđǩĒąĤøøøøŁĽņļƦĺŘź
```

###Benefits

1. This offset was chosen because every character in the range 248-503 is
recognized as a character and not a separator. Thus, when you print out the
encoding, you can double click the text and the entire row will highlight. This
makes for easy copy and pasting of data.

2. The character length of the encoded string is the byte length of the binary
data. This can be helpful when you are character constrained (like Twitter)
rather than byte constrained and base64 or base58 encoding is too large.

3. Encoders and decoders are super simple. No splitting bytes or bitwise
operations. Just an integer offest.

###Full Character Map

```
øùúûüýþÿĀāĂăĄąĆćĈĉĊċČčĎďĐđĒēĔĕĖėĘęĚěĜĝĞğĠġĢģĤĥĦħĨĩĪīĬĭĮįİıĲĳĴĵĶķĸĹĺĻļĽľĿŀŁłŃńŅņŇňŉŊŋŌōŎŏŐőŒœŔŕŖŗŘřŚśŜŝŞşŠšŢţŤťŦŧŨũŪūŬŭŮůŰűŲųŴŵŶŷŸŹźŻżŽžſƀƁƂƃƄƅƆƇƈƉƊƋƌƍƎƏƐƑƒƓƔƕƖƗƘƙƚƛƜƝƞƟƠơƢƣƤƥƦƧƨƩƪƫƬƭƮƯưƱƲƳƴƵƶƷƸƹƺƻƼƽƾƿǀǁǂǃǄǅǆǇǈǉǊǋǌǍǎǏǐǑǒǓǔǕǖǗǘǙǚǛǜǝǞǟǠǡǢǣǤǥǦǧǨǩǪǫǬǭǮǯǰǱǲǳǴǵǶǷ
```

###Example Encoders/Decoders

####Python

```python
def encode(input):
    return u"".join(unichr(b + 248) for b in input) #use chr() for Python 3

def decode(input):
    return [ord(c) - 248 for c in input]

test = [1,3,3,7]
encoded = encode(test) #u"ùûûÿ"
decoded = decode(encoded)
print u"[1,3,3,7] => \"{}\", test={}".format(encoded, test == decoded)
```

####Javascript

```javascript
function encode(input) {
    var output = "";
    for(var i = 0; i < input.length; i++) {
        output += String.fromCharCode(input[i] + 248);
    }
    return output;
}

function decode(input) {
    var output = [];
    for(var i = 0; i < input.length; i++){
        output.push(input.charCodeAt(i) - 248);
    }
    return output;
}

var test = [1,3,3,7]; //can be Uint8Array, too
var encoded = encode(test); //"ùûûÿ"
var decoded = decode(encoded);
console.log("[1,3,3,7] => \"%s\", test=%s", encoded, test.toString() === decoded.toString());
```

**Pull Requests Wanted! Add an encoder and decoder for your favorite language**

###License

All documentation and example code is released as public domain.

