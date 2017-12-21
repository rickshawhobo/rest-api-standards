# rest-api-standards
Draft 1.0.0


An API is a contract between an authority and a consumer. It is stateless and assumes no previous relationships. An API is uni-directional; the consumer makes requests to the authority and the authority answers. This specification is meant to serve as a framework to build the contractual relationship between the consumer and the authority. Anything not defined here can be defined as separate standards between the authority and the consumer as long as that contract does not violate this specification.



The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14  RFC2119  RFC8174 when, and only when, they appear in all capitals, as shown here.



Section 1: Basic Definitions
endpoint - a URI representing a resource or collection of resources

modifier - parameters that modify an endpoint, usually given as query parameters

resource - a single object or noun

collection - many of the same resources grouped together

id - a unique identifier for a resource

Section 2: Verbs
The following verbs are borrowed from RFC2616 and are used to interact with resources and collections

GET - retrieve a resource or collection

POST - creates a resource

PUT - replace an entire resource but keep the resource id

DELETE - remove a resource

PATCH - update a resource



Section 3: Status Codes
The following status codes are borrowed from RFC2616

Each code MUST have only one definition and MUST NOT represent any other definition

200 - Ok - generic success. MAY be used for successful retrieval of resources or collections. Successful deletion of a resource. Successful updates to a resource, and successful creation of a resource

301 - Moved - a resource or collection is known but has moved

400 - Creation or updating of a resource has failed, more commonly known as validation errors

401 - Unauthorized - the user has not been identified and the endpoint requires that the user be identified

403 - Forbidden - the user is identified but lacks permissions to act on an endpoint

404 - A resource is known but the identifier was incorrect or has been deleted

501 - an endpoint is missing - the client should not try the request again

500 - Generic error - the client should try the request again

Section 4: Naming Resources
Resources SHOULD be named as nouns. If resources are named as nouns it MUST be named in the pluralized form of the noun.

Compound resources SHOULD be named with dash in place of a space

GET /swimming-pools/

GET /check-ins/



Section 5: Endpoint Structure
Endpoints for a singular resource MUST follow the structure RESOURCE_NAME/RESOURCE_IDENTIFIER

Endpoints for a collection MUST follow the structure RESOURCE_NAME/

Endpoints MAY be modified by appending parameters after a question mark ( ? )





Section 6: Nested Endpoints
Nesting resources is allowed but discouraged

Endpoints MAY be combined to represent nested resources

Endpoints representing a nested singular resource MUST follow the structure PARENT_RESOURCE/PARENT_ID/NESTED_RESOURCE/NESTED_ID

Endpoints representing a nested collection MUST follow the structure PARENT_RESOURCE/PARENT_ID/NESTED_RESOURCE/



Section 7: Retrieval Data Envelope
All data resource and collection retrieval MUST return data inside a data envelope. In JSON it would look like



GET /countries/US

{
    "data": {
        "id": "US",
        "population": 300000000,
        "founded": 1776
    }
}


GET /countries/

{
    "data": [
    {
        "id": "US",
        "population": 300000000,
        "founded": 1776
    },
    {
        "id": "CA",
        "population": 36000000,
        "founded": 1867
    }
    ]
}




Section 8: Empty Collections
If a collection is known but contains no resources, it MUST return a successful empty colleciton

GET /puppies/



STATUS: 200

{
    "data": []
}




Section 9: Pagination in Retrieval of Collections
Collections MAY return meta data about the collection



{
    "data":
        [
        {
            "id": 123
        },
        {
            "id": 124
        }
        ]
    "total": 1000
}
Section 9.1: Controlling Collection Pagination
Controlling the collection pagination SHOULD be done via an endpoint modifier

GET /products/?page[current]=3&page[size]=100





Section 10: Resource Property Naming
Resource properties MUST be named in lowercase

Resource properites MUST be named in camel case style in place of a space

{
    "id": 123,
    "firstName": "Bob",
    "currentAge": 32,
    "lat": "48.669",
    "long": "-4.329",
    "geoHash": "gbsuv"
}




Section 11: Other Recommendations
Filtering collections SHOULD be done via an endpoint modifier

GET /cities/?filter[name]=New&filter[population]=3m



Sorting collections SHOULD be done via an endpoint modifier

GET /patients/?sort[risk]=desc&sort[currentAge]=asc

