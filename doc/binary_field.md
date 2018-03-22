## Binary field

Extension provides `json` object a new ability to store binary data using `std::vector<uint8_t>` type dedicated for use with `json.hpp` Message Pack serializer and parser.


'''Note''': binary field is not compatible with json serializer and parser. Do not use binary field with `dump` functions.


###Example:
```cpp
#include "json.hpp"
#include <vector>

...

using namespace nlohmann;
using namespace std;

...

/// sample random binary data 
const unsigned int N = 1024;
std::vector<uint8_t> binaryData(N);
for (int i = 0; i < N; i++) {
    binaryData[i] = rand();
}

/// assign data to json object by constructor
json j(binaryData);
json j2 = binaryData;

/// assign as a named field
json j3;
j3["f1"] = "test";
j3["f2"] = binaryData;

/// serialize deserialize using Message Pack
vector<uint8_t> packed = json::to_msgpack(jsonObj);
json restoredJson = json::from_msgpack(packed);
std::vector<uint8_t> restoredData = restoredJson.at("f2");

/// do not:
//jsonObj.dump(); // cannot dump json object being or containg binary field

//jsonObj.get<std::valarray<std::vector<uint8_t>>>();         // invalid conversion
//jsonObj.get<std::unordered_set<std::vector<uint8_t>>>();    // invalid conversion

// conversion from unsigned char arrays will not yeld to binary field  
//unsigned char array[ ] = {'a', 'c'};
//json notBinary2 = array;       //will be json array
//json notBinary1 = {'a', 'c'};  //will be json array

// conversion to unordered_set is not working    
//std::unordered_set<json> example {json("string"), json(), json(vector<uint8_t>({0x00,0x33})};    //invalid conversion
```