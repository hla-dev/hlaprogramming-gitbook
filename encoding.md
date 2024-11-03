# Data Encoding

Data is communicated between federates in an encoded form. The HLA defines a
number of standard encodings in IEEE 1516.2-2010 Object Model Template and
specifies the existence of encoding helper classes in the programming language
APIs described in IEEE 1516.1 Interface Specification. This page describes how
to use these encoding helper classes.

## Java Encoding Helpers

The encoding helper routines are all present within the `hla.rti1516e.encoding`
package of the Java API. The first step is to retrieve the `EncoderFactory` from
the RTI being used.

```java
import hla.rti1516e.RtiFactoryFactory;
import hla.rti1516e.RtiFactory;
import hla.rti1516e.encoding.EncoderFactory;

public class Main {
  public static void main(String[] args) {
    final var rtiFactory = RtiFactoryFactory.getRtiFactory();
    final var encoderFactory = rtiFactory.getEncoderFactory();
  }
}
```

The `EncoderFactory` is an interface that provides methods for creating
instances of the _encoder classes_ that encapsulate the standard encoding rules
for the base datatypes/representations and the four constructed datatypes
defined in the IEEE 1516.2-2010 Object Model Template.

The encoder classes all implement the `DataElement` inteface which has the
`byte[] encode()` method to encode the contents of an encoder class instance
into a byte array. The `DataElement` inteface also has the
`void decode(byte[] data)` method for decoding a value from the supplied byte
array.

The encoder class to use depends on what information needs to be sent.

### Base Datatypes

The base datatypes align with the primitive types present in Java: `byte`,
`short`, `int`, `long`, `float`, and `double`. There are little endian and big
endian versions of each. Which are used is determined by the aggrement made
between federates and captured in the Federation Object Model (FOM).

Encoding values for the base datatypes is relatively simple. For example, the
following encodes a big endian integer.

```java
...
final var encoderFactory = rtiFactory.getEncoderFactory();
final var integerValue = ...;
final HLAinteger32BE integerEncoder = encoderFactory.createHLAinteger32BE(integerValue);
final byte[] encodedInteger = integerEncoder.encode();
```

You may not want to instantiate the encoder class every time you need, but
instead create it once and reuse it.

```java
final var encoderFactory = ...;
final HLAfloat64BE doubleEncoder = EncoderFactory.createHLAfloat64BE();
...
doubleEncoder.setValue(someDoubleVariable);
doubleEncoder.encode(); // returns byte[]
...
doubleEncoder.setvalue(someOtherDoubleValue);
doubleEncoder.encode(); // returns byte[]
```

To decode the base datatypes, we use the same classes as for encoding:

```java
final var encoderFactory = ...;
final HLAinteger32BE integerDecoder = encoderFactory.createHLAinteger32BE();
final byte[] encodedInteger = ...; // e.g., received from the federation
integerDecoder.decode(encodedInteger);
final var decodedInteger = integerDecoder.getValue();
```

Note here that the _encoder classes_ are used for both encoding and decoding, so
name your variables accordingly.

### Constructed Datatypes

Encoding helper classes exist for the four constructed datatypes defined by the
HLA: fixed length arrays, variable length arrays, fixed records, and variant
records.

#### Fixed Length Arrays

A federate receiving a fixed length array expects an exact number of elements
all of the same type. For example, to encode an array of five integers:

```java
final var encoderFactory = ...;
final HLAfixedArray<HLAinteger32BE> integerFixedArrayEncoder =
  encoderFactory.createHLAfixedArray(
    encoderFactory.createHLAinteger32BE(intValue1),
    encoderFactory.createHLAinteger32BE(intValue2),
    encoderFactory.createHLAinteger32BE(intValue3),
    encoderFactory.createHLAinteger32BE(intValue4),
    encoderFactory.createHLAinteger32BE(intValue5)
  );
final byte[] encodedFixedArray = integerFixedArrayEncoder.encode();
```

Decoding is a bit more complex. It requires the definition of a
`DataElementFactory` that can be called during the decoding process to create
the individual elements of the fixed array. The exemplar API for
`DataElementFactory` includes a code snipped for how it can be used.

```java
final var encoderFactory = ...;
DataElementFactory factory = new DataElementFactory() {
  public DataElement createElement(int index) {
    return encoderFactory.createHLAinteger32BE();
  }
};

HLAfixedArray integerFixedArrayDecoder = encoderFactory.createHLAfixedArray(factory, 5);
integerFixedArrayDecoder.decode(bytes);
```
