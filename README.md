Overview
--------
This is a lightweight, minimalistic Zotero API client written in JavaScript. It's been developed based on the following principles:

* Small, single purpose module, i.e. talk to the API
* Works in node & browser (with the help of babel & commonjs)
* No abstraction over Zotero data, what you see is what you get
* Clean api
* Minimal request validation
* Predictable and consistent responses
* Great test coverage, testing of all features

**Bear in mind it doesn't do any of the following:**

* Version management - version headers need to be provided explictely
* Caching - each call to get(), post() etc. will actually call the repo
* Abstraction - There is no **Item** or **Collection**

This library should be considered a low level tool to talk to the API. For more clever, high level API client with abstraction over data see [libZotero](https://github.com/fcheslack/libZoteroJS)

Getting The Library
-------------------

	npm i zoterojs


Quick Start
-----------

Quick example, read from the api in three lines of code (we're using [async functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) from ES2017)

	// require the library
	const api = require('zoterojs');

	// use the api to make an async request and wait for
	// the promise to resolve
	const items = await api().library('user', 475425).collections('9KH9TNSJ').items().get().getData();
	console.log(items.map(i => i.title));


Overview
--------

Library composes of three layers:

* An api function, which is the only interface exported.
* A request engine called by the api. It does the heavy lifting.
* An ApiResponse, or, more likely, its specialised variant


Documentation
-------------

## Modules

<dl>
<dt><a href="#module_api">api</a></dt>
<dd><p>Module contains api() function, a Zotero API client</p>
</dd>
<dt><a href="#module_request">request</a></dt>
<dd><p>Module contains a request() function, a low-level Zotero API client</p>
</dd>
<dt><a href="#module_response">response</a></dt>
<dd><p>Module contains classes that offer abstraction over Zotero API responses</p>
</dd>
</dl>

<a name="module_api"></a>

## api
Module contains api() function, a Zotero API client


* [api](#module_api)
    * [~api(key, opts)](#module_api..api) ⇒ <code>Object</code>
    * [~library([typeOrKey], [id])](#module_api..library) ⇒ <code>Object</code>
    * [~items(items)](#module_api..items) ⇒ <code>Object</code>
    * [~itemTypes()](#module_api..itemTypes) ⇒ <code>Object</code>
    * [~itemFields()](#module_api..itemFields) ⇒ <code>Object</code>
    * [~creatorFields()](#module_api..creatorFields) ⇒ <code>Object</code>
    * [~itemTypeFields(itemType)](#module_api..itemTypeFields) ⇒ <code>Object</code>
    * [~itemTypeCreatorTypes(itemType)](#module_api..itemTypeCreatorTypes) ⇒ <code>Object</code>
    * [~template(itemType)](#module_api..template) ⇒ <code>Object</code>
    * [~collections(items)](#module_api..collections) ⇒ <code>Object</code>
    * [~tags(tags)](#module_api..tags) ⇒ <code>Object</code>
    * [~searches(searches)](#module_api..searches) ⇒ <code>Object</code>
    * [~top()](#module_api..top) ⇒ <code>Object</code>
    * [~trash()](#module_api..trash) ⇒ <code>Object</code>
    * [~children()](#module_api..children) ⇒ <code>Object</code>
    * [~version(version)](#module_api..version) ⇒ <code>Object</code>
    * [~get(opts)](#module_api..get) ⇒ <code>Promise</code>
    * [~post(data, opts)](#module_api..post) ⇒ <code>Promise</code>
    * [~put(data, opts)](#module_api..put) ⇒ <code>Promise</code>
    * [~patch(data, opts)](#module_api..patch) ⇒ <code>Promise</code>
    * [~del(keysToDelete, opts)](#module_api..del) ⇒ <code>Promise</code>

<a name="module_api..api"></a>

### api~api(key, opts) ⇒ <code>Object</code>
Entry point of the interface. Configures authentication.
Can be used to configure any other properties of the api
Returns a set of function that are bound to that configuration
and can be called to specify further api configuration.

**Kind**: inner method of [<code>api</code>](#module_api)  
**Chainable**  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| key | <code>String</code> | Authentication key |
| opts | <code>Object</code> | Optional api configuration. For a list of all                         possible properties, see documentation for                         request() function |

<a name="module_api..library"></a>

### api~library([typeOrKey], [id]) ⇒ <code>Object</code>
Configures which library api requests should use.

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| [typeOrKey] | <code>\*</code> | Library key, e.g. g1234. Alternatively, if                          second parameter is present, library type i.e                          either 'group' or 'user' |
| [id] | <code>Number</code> | Only when first argument is a type, library id |

<a name="module_api..items"></a>

### api~items(items) ⇒ <code>Object</code>
Configures api to use items or a specific item
Can be used in conjuction with library(), collections(), top(), trash(),
children(), tags() and any execution function (e.g. get(), post())

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| items | <code>String</code> | Item key, if present, configure api to point at                          this specific item |

<a name="module_api..itemTypes"></a>

### api~itemTypes() ⇒ <code>Object</code>
Configure api to request all item types
Can only be used in conjuction with get()

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  
<a name="module_api..itemFields"></a>

### api~itemFields() ⇒ <code>Object</code>
Configure api to request all item fields
Can only be used in conjuction with get()

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  
<a name="module_api..creatorFields"></a>

### api~creatorFields() ⇒ <code>Object</code>
Configure api to request localized creator fields
Can only be used in conjuction with get()

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  
<a name="module_api..itemTypeFields"></a>

### api~itemTypeFields(itemType) ⇒ <code>Object</code>
Configure api to request all valid fields for an item type
Can only be used in conjuction with get()

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| itemType | <code>String</code> | item type for which valid fields will be                             requested, e.g. 'book' or 'journalType' |

<a name="module_api..itemTypeCreatorTypes"></a>

### api~itemTypeCreatorTypes(itemType) ⇒ <code>Object</code>
Configure api to request valid creator types for an item type
Can only be used in conjuction with get()

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| itemType | <code>String</code> | item type for which valid creator types                             will be requested, e.g. 'book' or                              'journalType' |

<a name="module_api..template"></a>

### api~template(itemType) ⇒ <code>Object</code>
Configure api to request template for a new item
Can only be used in conjuction with get()

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| itemType | <code>String</code> | item type for which template will be                             requested, e.g. 'book' or 'journalType' |

<a name="module_api..collections"></a>

### api~collections(items) ⇒ <code>Object</code>
Configure api to use collections or a specific collection
Can be used in conjuction with library(), items(), top(), tags() and
any of the execution function (e.g. get(), post())

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| items | <code>String</code> | Collection key, if present, configure api to                          point to this specific collection |

<a name="module_api..tags"></a>

### api~tags(tags) ⇒ <code>Object</code>
Configure api to request or delete tags or request a specific tag
Can be used in conjuction with library(), items(), collections() and
any of the following execution functions: get(), delete() but only
if the first argument is not present. Otherwise can only be used in
conjuctin with get()

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| tags | <code>String</code> | name of a tag to request. If preset, configure                         api to request specific tag. |

<a name="module_api..searches"></a>

### api~searches(searches) ⇒ <code>Object</code>
Configure api to use saved searches or a specific saved search
Can be used in conjuction with library() and any of the execution
functions

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| searches | <code>String</code> | Search key, if present, configure api to point at                             this specific saved search |

<a name="module_api..top"></a>

### api~top() ⇒ <code>Object</code>
Configure api to narrow the request only to the top level items
Can be used in conjuction with items() and collections() and only
with conjuction with a get() execution function

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  
<a name="module_api..trash"></a>

### api~trash() ⇒ <code>Object</code>
Configure api to narrow the request only to the items in the trash
Can be only used in conjuction with items() and get() execution
function

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  
<a name="module_api..children"></a>

### api~children() ⇒ <code>Object</code>
Configure api to narrow the request only to the children of given
item
Can be only used in conjuction with items() and get() execution
function

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  
<a name="module_api..version"></a>

### api~version(version) ⇒ <code>Object</code>
Configure api to specify local version of given entity.
When used in conjuction with get() exec function, it will populate the
If-Modified-Since-Version header.
When used in conjuction with post(), put(), patch() or delete() it will
populate the If-Unmodified-Since-Version header.

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Object</code> - - Partially configured api functions  

| Param | Type | Description |
| --- | --- | --- |
| version | <code>Number</code> | local version of the entity |

<a name="module_api..get"></a>

### api~get(opts) ⇒ <code>Promise</code>
Execution function. Specifies that the request should use a GET method.

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Promise</code> - - Returns a promise that will eventually return
                        either an ApiResponse, SingleReadResponse or
                        MultiReadResponse. Might throw Error or 
                        ErrorResponse  

| Param | Type | Description |
| --- | --- | --- |
| opts | <code>Object</code> | Optional api configuration. If duplicate,                          overrides properties already present. For a list                         of all possible properties, see documentation                         for request() function |

<a name="module_api..post"></a>

### api~post(data, opts) ⇒ <code>Promise</code>
Execution function. Specifies that the request should use a POST method.

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Promise</code> - - Returns a promise that will eventually return
                        either an MultiWriteResponse. Might throw Error
                        or ErrorResponse  

| Param | Type | Description |
| --- | --- | --- |
| data | <code>Array</code> | An array of entities to post |
| opts | <code>Object</code> | Optional api configuration. If duplicate,                          overrides properties already present. For a list                         of all possible properties, see documentation                         for request() function |

<a name="module_api..put"></a>

### api~put(data, opts) ⇒ <code>Promise</code>
Execution function. Specifies that the request should use a PUT method.

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Promise</code> - - Returns a promise that will eventually return
                        either an SingleWriteResponse. Might throw Error
                        or ErrorResponse  

| Param | Type | Description |
| --- | --- | --- |
| data | <code>Object</code> | An entity to put |
| opts | <code>Object</code> | Optional api configuration. If duplicate,                          overrides properties already present. For a list                         of all possible properties, see documentation                         for request() function |

<a name="module_api..patch"></a>

### api~patch(data, opts) ⇒ <code>Promise</code>
Execution function. Specifies that the request should use a PATCH
method.

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Promise</code> - - Returns a promise that will eventually return
                        either an SingleWriteResponse. Might throw Error
                        or ErrorResponse  

| Param | Type | Description |
| --- | --- | --- |
| data | <code>Object</code> | Partial entity data to patch |
| opts | <code>Object</code> | Optional api configuration. If duplicate,                          overrides properties already present. For a list                         of all possible properties, see documentation                         for request() function |

<a name="module_api..del"></a>

### api~del(keysToDelete, opts) ⇒ <code>Promise</code>
Execution function. Specifies that the request should use a DELETE
method.

**Kind**: inner method of [<code>api</code>](#module_api)  
**Returns**: <code>Promise</code> - - Returns a promise that will eventually return
                        either an DeleteResponse. Might throw Error
                        or ErrorResponse  

| Param | Type | Description |
| --- | --- | --- |
| keysToDelete | <code>Array</code> | An array of keys to delete. Depending on                                how api has been configured, these will                                be item keys, collection keys, search                                 keys or tag names. If not present, api                                should be configured to use specific                                 item, collection or saved search, in                                which case, that entity will be deleted |
| opts | <code>Object</code> | Optional api configuration. If duplicate,                          overrides properties already present. For a list                         of all possible properties, see documentation                         for request() function |

<a name="module_request"></a>

## request
Module contains a request() function, a low-level Zotero API client

<a name="module_request..request"></a>

### request~request() ⇒ <code>Object</code>
Executes request and returns a response

**Kind**: inner method of [<code>request</code>](#module_request)  
**Returns**: <code>Object</code> - Returns a Promise that will eventually return a response object  
**Throws**:

- <code>Error</code> If options specify impossible configuration
- <code>ErrorResponse</code> If API responds with a non-ok response


| Param | Type | Description |
| --- | --- | --- |
| options.authorization | <code>String</code> | 'Authorization' header |
| options.zoteroWriteToken | <code>String</code> | 'Zotero-Write-Token' header |
| options.ifModifiedSinceVersion | <code>String</code> | 'If-Modified-Since-Version' header |
| options.ifUnmodifiedSinceVersion | <code>String</code> | 'If-Unmodified-Since-Version' header |
| options.contentType | <code>String</code> | 'Content-Type' header |
| options.format | <code>String</code> | 'format' query argument |
| options.include | <code>String</code> | 'include' query argument |
| options.content | <code>String</code> | 'content' query argument |
| options.style | <code>String</code> | 'style' query argument |
| options.itemKey | <code>String</code> | 'itemKey' query argument |
| options.collectionKey | <code>String</code> | 'collectionKey' query argument |
| options.searchKey | <code>String</code> | 'searchKey' query argument |
| options.itemType | <code>String</code> | 'itemType' query argument |
| options.qmode | <code>String</code> | 'qmode' query argument |
| options.since | <code>Number</code> | 'since' query argument |
| options.tag | <code>String</code> | 'tag' query argument |
| options.sort | <code>String</code> | 'sort' query argument |
| options.direction | <code>String</code> | 'direction' query argument |
| options.limit | <code>Number</code> | 'limit' query argument |
| options.start | <code>Number</code> | 'start' query argument |
| options.resource.top | <code>String</code> | use 'top' resource |
| options.resource.trash | <code>String</code> | use 'trash' resource |
| options.resource.children | <code>String</code> | use 'children' resource |
| options.resource.groups | <code>String</code> | use 'groups' resource |
| options.resource.itemTypes | <code>String</code> | use 'itemTypes' resource |
| options.resource.itemFields | <code>String</code> | use 'itemFields' resource |
| options.resource.creatorFields | <code>String</code> | use 'creatorFields' resource |
| options.resource.itemTypeFields | <code>String</code> | use 'itemTypeFields' resource |
| options.resource.itemTypeCreatorTypes | <code>String</code> | use 'itemTypeCreatorTypes' resource |
| options.resource.library | <code>String</code> | use 'library' resource |
| options.resource.collections | <code>String</code> | use 'collections' resource |
| options.resource.items | <code>String</code> | use 'items' resource |
| options.resource.searches | <code>String</code> | use 'searches' resource |
| options.resource.tags | <code>String</code> | use 'tags' resource |
| options.resource.template | <code>String</code> | use 'template' resource |
| options.method | <code>String</code> | forwarded to fetch() |
| options.body | <code>String</code> | forwarded to fetch() |
| options.mode | <code>String</code> | forwarded to fetch() |
| options.cache | <code>String</code> | forwarded to fetch() |
| options.credentials | <code>String</code> | forwarded to fetch() |

<a name="module_response"></a>

## response
Module contains classes that offer abstraction over Zotero API responses


* [response](#module_response)
    * [~SingleReadResponse](#module_response..SingleReadResponse) ⇐ <code>ApiResponse</code>
        * [.getData()](#module_response..SingleReadResponse+getData) ⇒ <code>Object</code>
    * [~MultiReadResponse](#module_response..MultiReadResponse) ⇐ <code>ApiResponse</code>
        * [.getData()](#module_response..MultiReadResponse+getData) ⇒ <code>Array</code>
    * [~SingleWriteResponse](#module_response..SingleWriteResponse) ⇐ <code>ApiResponse</code>
        * [.getData()](#module_response..SingleWriteResponse+getData) ⇒ <code>Object</code>
    * [~MultiWriteResponse](#module_response..MultiWriteResponse) ⇐ <code>ApiResponse</code>
        * [.isSuccess()](#module_response..MultiWriteResponse+isSuccess) ⇒ <code>Boolean</code>
        * [.getData()](#module_response..MultiWriteResponse+getData) ⇒ <code>Array</code>
        * [.getErrors()](#module_response..MultiWriteResponse+getErrors) ⇒ <code>Object</code>
        * [.getEntityByKey(key)](#module_response..MultiWriteResponse+getEntityByKey)
        * [.getEntityByIndex(index)](#module_response..MultiWriteResponse+getEntityByIndex) ⇒ <code>Object</code>
    * [~DeleteResponse](#module_response..DeleteResponse) ⇐ <code>ApiResponse</code>
    * [~ErrorResponse](#module_response..ErrorResponse) ⇐ <code>Error</code>

<a name="module_response..SingleReadResponse"></a>

### response~SingleReadResponse ⇐ <code>ApiResponse</code>
represents a response to a GET request containing a single entity

**Kind**: inner class of [<code>response</code>](#module_response)  
**Extends**: <code>ApiResponse</code>  
<a name="module_response..SingleReadResponse+getData"></a>

#### singleReadResponse.getData() ⇒ <code>Object</code>
**Kind**: instance method of [<code>SingleReadResponse</code>](#module_response..SingleReadResponse)  
**Returns**: <code>Object</code> - entity returned in this response  
<a name="module_response..MultiReadResponse"></a>

### response~MultiReadResponse ⇐ <code>ApiResponse</code>
represnets a response to a GET request containing multiple entities

**Kind**: inner class of [<code>response</code>](#module_response)  
**Extends**: <code>ApiResponse</code>  
<a name="module_response..MultiReadResponse+getData"></a>

#### multiReadResponse.getData() ⇒ <code>Array</code>
**Kind**: instance method of [<code>MultiReadResponse</code>](#module_response..MultiReadResponse)  
**Returns**: <code>Array</code> - a list of entities returned in this response  
<a name="module_response..SingleWriteResponse"></a>

### response~SingleWriteResponse ⇐ <code>ApiResponse</code>
represents a response to a PUT or PATCH request

**Kind**: inner class of [<code>response</code>](#module_response)  
**Extends**: <code>ApiResponse</code>  
<a name="module_response..SingleWriteResponse+getData"></a>

#### singleWriteResponse.getData() ⇒ <code>Object</code>
**Kind**: instance method of [<code>SingleWriteResponse</code>](#module_response..SingleWriteResponse)  
**Returns**: <code>Object</code> - For put requests, this represents a complete, updated object.
                 For patch requests, this reprents only updated fields of the updated object.  
<a name="module_response..MultiWriteResponse"></a>

### response~MultiWriteResponse ⇐ <code>ApiResponse</code>
represents a response to a POST request

**Kind**: inner class of [<code>response</code>](#module_response)  
**Extends**: <code>ApiResponse</code>  

* [~MultiWriteResponse](#module_response..MultiWriteResponse) ⇐ <code>ApiResponse</code>
    * [.isSuccess()](#module_response..MultiWriteResponse+isSuccess) ⇒ <code>Boolean</code>
    * [.getData()](#module_response..MultiWriteResponse+getData) ⇒ <code>Array</code>
    * [.getErrors()](#module_response..MultiWriteResponse+getErrors) ⇒ <code>Object</code>
    * [.getEntityByKey(key)](#module_response..MultiWriteResponse+getEntityByKey)
    * [.getEntityByIndex(index)](#module_response..MultiWriteResponse+getEntityByIndex) ⇒ <code>Object</code>

<a name="module_response..MultiWriteResponse+isSuccess"></a>

#### multiWriteResponse.isSuccess() ⇒ <code>Boolean</code>
**Kind**: instance method of [<code>MultiWriteResponse</code>](#module_response..MultiWriteResponse)  
**Returns**: <code>Boolean</code> - Indicates whether all write operations were successful  
<a name="module_response..MultiWriteResponse+getData"></a>

#### multiWriteResponse.getData() ⇒ <code>Array</code>
Returns all entities POSTed in an array. Entities that have been written successfully
are returned updated, other entities are returned unchanged. It is advised to verify
if request was entirely successful (see isSuccess and getError) before using this method.

**Kind**: instance method of [<code>MultiWriteResponse</code>](#module_response..MultiWriteResponse)  
**Returns**: <code>Array</code> - A modified list of all entities posted.  
<a name="module_response..MultiWriteResponse+getErrors"></a>

#### multiWriteResponse.getErrors() ⇒ <code>Object</code>
Returns all errors that have occurred.

**Kind**: instance method of [<code>MultiWriteResponse</code>](#module_response..MultiWriteResponse)  
**Returns**: <code>Object</code> - Errors object where keys are indexes of the array of the original request and values are the erorrs occurred.  
<a name="module_response..MultiWriteResponse+getEntityByKey"></a>

#### multiWriteResponse.getEntityByKey(key)
Allows obtaining updated entity based on its key, otherwise identical to getEntityByIndex

**Kind**: instance method of [<code>MultiWriteResponse</code>](#module_response..MultiWriteResponse)  
**Throws**:

- <code>Error</code> If key is not present in the request

**See**: [getEntityByIndex](getEntityByIndex)  

| Param | Type |
| --- | --- |
| key | <code>String</code> | 

<a name="module_response..MultiWriteResponse+getEntityByIndex"></a>

#### multiWriteResponse.getEntityByIndex(index) ⇒ <code>Object</code>
Allows obtaining updated entity based on its index in the original request

**Kind**: instance method of [<code>MultiWriteResponse</code>](#module_response..MultiWriteResponse)  
**Throws**:

- <code>Error</code> If index is not present in the original request
- <code>Error</code> If error occured in the POST for selected entity. Error message will contain reason for failure.


| Param | Type |
| --- | --- |
| index | <code>Number</code> | 

<a name="module_response..DeleteResponse"></a>

### response~DeleteResponse ⇐ <code>ApiResponse</code>
represents a response to a DELETE request

**Kind**: inner class of [<code>response</code>](#module_response)  
**Extends**: <code>ApiResponse</code>  
<a name="module_response..ErrorResponse"></a>

### response~ErrorResponse ⇐ <code>Error</code>
represents an error response from the api

**Kind**: inner class of [<code>response</code>](#module_response)  
**Extends**: <code>Error</code>  
**Properties**

| Name | Type | Description |
| --- | --- | --- |
| response | <code>Object</code> | Response object for the request, with untouched body |
| message | <code>String</code> | What error occurred, ususally contains response code and status |
| reason | <code>String</code> | More detailed reason for the failure, if provided by the API |
| options | <code>String</code> | Configuration object used for this request |

