# Connecting to an RTI

The first task is to connect to a Runtime Infrastructure \(RTI\). This requires
the creation of an RTI Ambassador and the actual connection.

## Pseudocode

1. Create an RTI Ambassador
2. Create a Federate Ambassador
3. Use the RTI Ambassador to 'connect' to the RTI
4. Your code
5. Use the RTI Ambassador to 'disconnect' from the RTI

## C++

```cpp
#include "RTI/RTI1516.h"

int main(int argc, char **argv) {
    // 1. Create an RTI Ambassador
    auto rtiAmbassadorFactory = new rti1516e::RTIambassadorFactory();
    auto rtiAmbassador = rtiAmbassdorFactory.createRTIambassador();

    // 2. Create a Federate Ambassador
    rti1516e::NullFederateAmbassador fedAmbassador;

    // 3. Connect to an RTI
    rtiAmbassador->connect(
        fedAmbassador,
        rti1516e::HLA_EVOKED
    );

    // 4. Your code

    // 5. Disconnect from the RTI
    rtiAmbassador->disconnect();

    return 0;
}
```

## Java

```java
import hla.rti1516e.RtiFactoryFactory;
import hla.rti1516e.RtiFactory;
import hla.rti1516e.RTIambassador;
import hla.rti1516e.FederateAmbassador;
import hla.rti1516e.NullFederateAmbassador;
import hla.rti1516e.CallbackModel;

public class SimulationApplication {
    public static void main(String[] args) {
        // 1. Create an RTI Ambassador
        RtiFactory rtiFactory = RtiFactoryFactory.getRtiFactory();
        RTIambassador rtiAmbassador = rtiFactory.getRtiAmbassador();

        // 2. Create a Federate Ambassador
        FederateAmbassador fedAmbassador =
            new NullFederateAmbassador();

        // 3. Connect to an RTI
        rtiAmbassador.connect(
            fedAmbassador,
            CallbackModel.HLA_EVOKED,
            ""
        );

        // 4. Your code

        // 5. Disconnect from the RTI
        rtiAmbassador.disconnect();
    }
}
```
