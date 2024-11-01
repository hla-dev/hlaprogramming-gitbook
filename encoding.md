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
for the base and simple datatypes and the four aggregate datatypes defined in
the IEEE 1516.2-2010 Object Model Template.

The encoder classes all implement the `DataElement` inteface which has the
`byte[] encode()` method to encode the contents of an encoder class instance
into a byte array.

The encoder class to use depends on what information needs to be sent. The base
datatypes align with the primitive types present in Java: `byte`, `short`,
`int`, `long`, `float`, and `double`. There are little endian and big endian
versions of each. Which are used is determined by the aggrement made between
federates and captured in the Federation Object Model (FOM).

Encoding base datatypes is relatively simple. For example, the following encodes
a big endian integer.

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

Before looking at the encoding process for the simple and aggregate datatypes,
we'll look at the decoding process, which is similar for all types.

```java
final var encoderFactory = ...;
final HLAinteger32BE integerDecoder = encoderFactory.createHLAinteger32BE();
final byte[] encodedInteger = ...; // e.g., received from the federation
integerDecoder.decode(encodedInteger);
final var decodedInteger = integerDecoder.getValue();
```

Note here that the _encoder classes_ are used for both encoding and decoding, so
name your variables accordingly.
