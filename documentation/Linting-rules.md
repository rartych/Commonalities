# CAMARA API Linting Rules Documentation

## Introduction


## 1. Spectral core ruleset

Spectral has a built-in OpenAPI Specification ruleset that can be used to validate OpenAPI files.

With `extends: "spectral:oas"` ("oas" being shorthand for OpenAPI Specification) in the ruleset file to rules for OpenAPI v2 and v3.x, depending on the appropriate OpenAPI version being used (this is automatically detected through formats) are added to the final ruleset.

The full list of the rules in this ruleset are described in [OpenAPI Rules](https://docs.stoplight.io/docs/spectral/4dec24461f3af-open-api-rules).


`extends: [[spectral:oas, off]]`  - this avoids running any rules from the extended ruleset as they are disabled. Each rule can [enabled individually](https://docs.stoplight.io/docs/spectral/0a73453054745-recommended-or-all#enabling-rules).

### Rule severity

The `severity` keyword is optional in rule definition and can be `error`, `warn`, `info`, `hint`, or `off`.
The default value is `warn`.

### OpenAPI v2 & v3
Rules applying to both OpenAPI v2.0, v3.0, and most likely v3.1.


|Name| Desc| Recom mended|CAMARA use|Spectral severity | CAMARA severity
|---|---|---|--|---|--|
|contact-properties| contact object is full of the most useful properties: `name`, `url`, `and email`|No|No |  |  |
|duplicated-entry-in-enum| Each value of an `enum` must be different from one another |Yes  | Yes |  |  |
|info-contact |Info object should contain `contact` object |Yes  | Yes|   |  |
|info-description |Info object should contain `description` object |Yes  | Yes|  |  | 
|info-license |Info object should contain `license` object |Yes  | Yes|   |  |
|license-url | link to the full text of licence | Yes | Yes|  |  |
|no-$ref-siblings| Before OpenAPI v3.1, keywords next to $ref were ignored | Yes | Yes|   |  |
|no-eval-in-markdown | injecting `eval()` JavaScript statements could lead to an XSS attack |Yes |Yes |  |  |
|no-script-tags-in-markdown |  injecting `<script>` tags could lead to execution of an arbitrary | Yes |Yes |  |  |
|openapi-tags | OpenAPI object should have non-empty `tags` array |??? | ??? (No) |   |  |
| openapi-tags-alphabetical | OpenAPI object should have alphabetical `tags` | No |No |  |  |
| openapi-tags-uniqueness | OpenAPI object must not have duplicated tag names | No | No |  |  |
|operation-description | ???| Yes| ???|  |  |
|operation-operationId | a reference for the operation - the value `lower-hyphen-case` **CAMARA: OperationIds are defined in `lowerCamelCase` For example: `helloWorld`** |Yes| Yes|  |  |
| operation-operationId-unique | Every operation must have a unique operationId | Yes| Yes|  |  |
| operation-operationId-valid-in-url | avoid non-URL-safe characters| Yes| Yes|  |  |
|operation-parameters | Operation parameters are unique and non-repeating | Yes| Yes|  |  |
| operation-singular-tag | Use just one tag for an operation | No| ???|  |  |
|operation-success-response | Operation must have at least one `2xx` or `3xx` response | Yes| Yes|  |  |
|operation-tags | Operation should have non-empty `tags` array| Yes| Yes|  |  |
| operation-tag-defined | Operation tags should be defined in global tags| Yes| Yes|  |  |
| path-declarations-must-exist | Path parameter declarations cannot be empty, ex.`/given/{}` is invalid.  | Yes| Yes|  |  |
| path-keys-no-trailing-slash | Keep trailing slashes off of paths,  | Yes| Yes|  |  |
| path-not-include-query | Don't put query string items in the path, they belong in parameters with `in: query`| Yes| Yes|  |  |
|path-params | Path parameters are correct and valid | Yes| Yes|  |  |
|tag-description | global tags have description | No | ???|  |  |
| typed-enum | Enum values should respect the type specifier. | Yes| Yes|  |  |
||||| |
|oas3-api-servers | OpenAPI servers must be present and non-empty array | Yes| Yes|  |  |
|oas3-examples-value-or-externalValue | Examples for requestBody or response examples can have an `externalValue` or a `value`, but they cannot have both| Yes| Yes|  |  |
|**oas3-operation-security-defined** | Operation `security` values must match a scheme defined in the `components.securitySchemes` object. | Yes| Yes|  |  |
|oas3-parameter-description | Parameter objects should have a description| No| Yes?|  |  |
| oas3-schema | Validate structure of OpenAPI v3 specification | Yes| Yes|  |  |
|oas3-server-not-example.com | Server URL should not point to *example.com*| No| Yes?|  |  |
|oas3-server-trailing-slash | Server URL should not have a trailing slash | Yes| Yes|  |  |
|oas3-unused-component | Potential unused reusable components entry has been detected  | Yes| Yes|  |  |
|oas3-valid-media-example | Examples must be valid against their defined schema. This rule is applied to *Media Type objects*  | Yes| Yes|  |  |
|oas3-valid-schema-example | Examples must be valid against their defined schema. This rule is applied to *Schema objects* | Yes| Yes|  |  |
|oas3-server-variables | This rule ensures that server variables defined in OpenAPI Specification 3 (OAS3) and 3.1 are valid, not unused | Yes| Yes|  |  |



## 2. Language
### Spell checking

CAMARA API specification files include inline documentation.

The description attributes should be checked for typos.

_Spectral rule_: [camara-language-spelling]()

*Severity*: `warn`

### Reduce telco-specific terminology in API definitions

API Design Guidelines: [2.5 Reduce telco-specific terminology in API definitions](https://github.com/camaraproject/Commonalities/blob/main/documentation/API-design-guidelines.md#25-reduce-telco-specific-terminology-in-api-definitions)

Consider and account for how the API can be fulfilled on a range of network types.
Avoid terms/types specific to a given telco domain -  terms should be inclusive beyond mobile network. 

See also [CAMARA Glossary](https://github.com/camaraproject/Commonalities/blob/main/documentation/Glossary.md)


| ❌ &nbsp; Not recommended | 👍  &nbsp; Recommended |
|----------------------------|-------------------------|
| `UE`                 | `device`           |
| `MSISDN`                 | `phone number`           |
| `mobile network`      | `network`         |


_Spectral rule_: [camara-language-avoid-telco]()

*Severity*: `hint`

## 3. API Definition
### Path parameters

API Design Guidelines: 
[3.4 Path Parameters Use](https://github.com/camaraproject/Commonalities/blob/main/documentation/API-design-guidelines.md#34-path-parameters-use)


Point 2 The attribute must be identifying itself, it is not enough with "{id}"

`/users/{id}`

| ❌ &nbsp; Not recommended | 👍  &nbsp; Recommended |
|----------------------------|-------------------------|
| `/users/{id}/documents/{documentId}`                 | `/users/{userId}/documents/{documentId}`           |

_Spectral rule_: [camara-path-param-id]()

*Severity*: `warn`

Point 3 The identifier should have a similar morphology on all endpoints. For example, “*xxxxId*”, where *xxx* is the name of the entity it reference

| 👍  &nbsp; Recommended |
|-------------------------|
| `/users/{userId}` |
|`/accounts/{accountId}` |
|`/vehicles/{vehicleId}` |
|`/users/{userId}/vehicles/{vehicleId}` |

_Spectral rule_: [camara-path-param-id-morphology]()

*Severity*: `warn`

