# Connecting to an RTI

The first task is to connect to a Runtime Infrastructure \(RTI\). This requires the creation of an RTI Ambassador and the actual connection.

## Pseudocode

```text
Create an RTI Ambassador
Create a Federate Ambassador
Use the RTI Ambassador to 'connect' to the RTI
// the rest of your code
Use the RTI Ambassador to 'disconnect' from the RTI
Delete the RTI Ambassador
```

## C++

```cpp
#include "RTI/RTI1516.h"

int main(int argc, char **argv) {
    auto rtiAmbassadorFactory = new rti1516e::RTIambassadorFactory();
    auto rtiAmbassador = rtiAmbassdorFactory.createRTIambassador();

    rti1516e::NullFederateAmbassador fedAmbassador;

    rtiAmbassador->connect(
        fedAmbassador,
        rti1516e::HLA_EVOKED
    );
    
    // the rest of your code
    
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
        RtiFactory rtiFactory = RtiFactoryFactory.getRtiFactory();
        RTIambassador rtiAmbassador = rtiFactory.getRtiAmbassador();
        
        FederateAmbassador fedAmbassador =
            new NullFederateAmbassador();
        rtiAmbassador.connect(
            fedAmbassador,
            CallbackModel.HLA_EVOKED,
            ""
        );

        // The rest of your code
        
        rtiAmbassador.disconnect();
    }
}
```





