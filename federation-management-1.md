# Federation Management

Federates have to be joined to a federation before they can exchange simulation
data. To join to a federation, the federation must first exist. Who creates the
federation is a design decision. One option is to have an annointed manager be
responsible for creating the federation. Another option is for all federates to
try to create the federation and handle the error should the federation already
exist. The code below uses this second approach.

Once your code is done, you resign from the federation. The federation is then
destroyed. Again, how the federation is destroyed is a design decision. Given
the decision above to have all federates attempt to create the federation, we
will also have all federates attempt to destroy the federation. The last
federate to resign from the federation will successfully destroy the federation.
All other federates will receive an exception stating that federates are still
joined to the federation.

## Pseudocode

1. Create the federation
2. Join the federation
3. Your code
4. Resign from the federation
5. Destroy the federation

## C++

```cpp
#include "RTI/RTI1516.h"

int main(int argc, char** argv) {
    // create RTI Ambassador instance and connect to RTI
    auto rtiamb = ...;

    auto federationName = std::wstring { L"federation-name" };
    auto fomModule = std::wstring { L"RPR-FOM.xml" };

    // 1. Create the federation
    try {
        rtiamb->createFederationExecution(federationName, fomModule);
    } catch (rti1516e::FederationExecutionAlreadyExists&) {
        // allow this exception and move on to join
    } catch (...) {
        // rethrow any other exceptions raised
        throw;
    }

    // 2. Join the federation
    auto federateName = std::wstring { L"federate-name" };
    auto federateType = std::wstring { L"federate-type" };

    rtiamb->joinFederationExecution(
        federateName,
        federateType,
        federationExecution
    );

    // 3. Your code

    // 4. Resign from the federation
    rtiamb->resignFederationExecution(rti1516e::DELETE_OBJECTS);

    // 5. Destroy federation execution
    try {
        rtiamb->destroyFederationExecution(federationName);
    } catch(rti1516e::FederatesCurrentlyJoined&) {
        // allow this exception
    } catch(...) {
        // rethrow all other exceptions
        throw;
    }

    return 0;
}
```
