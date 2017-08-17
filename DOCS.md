## Modules

<dl>
<dt><a href="#module_uport-js/JWT">uport-js/JWT</a></dt>
<dd></dd>
</dl>

## Classes

<dl>
<dt><a href="#Credentials">Credentials</a></dt>
<dd><p>Credentials allows you to easily created signed payloads used in uPort inlcuding
   credentials and signed mobile app requests (ex. selective disclosure requests for
   for private data). It also provides signature verification over mobile app response,
   helper functions and the ability to send push notifications to users.</p>
</dd>
</dl>

<a name="module_uport-js/JWT"></a>

## uport-js/JWT

* [uport-js/JWT](#module_uport-js/JWT)
    * [.createJWT([config], payload)](#module_uport-js/JWT.createJWT) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
    * [.verifyJWT([config], payload)](#module_uport-js/JWT.verifyJWT) ⇒ <code>Promise.&lt;Object, Error&gt;</code>

<a name="module_uport-js/JWT.createJWT"></a>

### uport-js/JWT.createJWT([config], payload) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
Creates a signed JWT given an address which becomes the issuer, a signer, and a payload for which the signature is over.

**Kind**: static method of <code>[uport-js/JWT](#module_uport-js/JWT)</code>  
**Returns**: <code>Promise.&lt;Object, Error&gt;</code> - a promise which resolves with a signed JSON Web Token or rejects with an error  

| Param | Type | Description |
| --- | --- | --- |
| [config] | <code>Object</code> | a unsigned credential object |
| config.address | <code>String</code> | address, typically the uPort address of the signer which becomes the issuer |
| config.signer | <code>SimpleSigner</code> | a signer, reference our SimpleSigner.js |
| payload | <code>Object</code> | data payload object |

**Example**  
```js
const signer = SimpleSigner(process.env.PRIVATE_KEY)
 createJWT({address: '5A8bRWU3F7j3REx3vkJ...', signer}, {key1: 'value', key2: ..., ... }).then(jwt => {
     ...
 })

 
```
<a name="module_uport-js/JWT.verifyJWT"></a>

### uport-js/JWT.verifyJWT([config], payload) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
Verifies given JWT. Registry is used to resolve uPort address to public key for verification.
 If the JWT is valid, the promise returns an object including the JWT, the payload of the JWT,
 and the profile of the issuer of the JWT.

**Kind**: static method of <code>[uport-js/JWT](#module_uport-js/JWT)</code>  
**Returns**: <code>Promise.&lt;Object, Error&gt;</code> - a promise which resolves with a signed JSON Web Token or rejects with an error  

| Param | Type | Description |
| --- | --- | --- |
| [config] | <code>Object</code> | a unsigned credential object |
| config.address | <code>String</code> | address, typically the uPort address of the signer which becomes the issuer |
| config.signer | <code>SimpleSigner</code> | a signer, reference our SimpleSigner.js |
| payload | <code>Object</code> | data payload object |

**Example**  
```js
const registry =  new UportLite()
 verifyJWT({registry, address: '5A8bRWU3F7j3REx3vkJ...'}, 'eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NksifQ.eyJyZXF1Z....').then(obj => {
     const payload = obj.payload
     const profile = obj.profile
     const jwt = obj.jwt
     ...
 })

 
```
<a name="Credentials"></a>

## Credentials
Credentials allows you to easily created signed payloads used in uPort inlcuding
   credentials and signed mobile app requests (ex. selective disclosure requests for
   for private data). It also provides signature verification over mobile app response,
   helper functions and the ability to send push notifications to users.

**Kind**: global class  

* [Credentials](#Credentials)
    * [new Credentials([settings])](#new_Credentials_new)
    * [.createRequest([params])](#Credentials+createRequest) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
    * [.receive(token, [callbackUrl])](#Credentials+receive) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
    * [.push(token)](#Credentials+push) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
    * [.attest([credential])](#Credentials+attest) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
    * [.lookup(address)](#Credentials+lookup) ⇒ <code>Promise.&lt;Object, Error&gt;</code>

<a name="new_Credentials_new"></a>

### new Credentials([settings])
Instantiates a new uPort Credentials object


| Param | Type | Description |
| --- | --- | --- |
| [settings] | <code>Object</code> | optional setttings |
| settings.networks | <code>Object</code> | networks config object, ie. {  '0x94365e3b': { rpcUrl: 'https://private.chain/rpc', address: '0x0101.... }} |
| settings.registry | <code>UportLite</code> | a registry object from UportLite |

**Example**  
```js
import { Credentials } from 'uport'
const registry =  new UportLite()
const networks = {  '0x94365e3b': { rpcUrl: 'https://private.chain/rpc', address: '0x0101.... }}
const setttings = { registry, networks }
const credentials = new Credentials(settings)
```
**Example**  
```js
import { Credentials } from 'uport'
const credentials = new Credentials()
```
<a name="Credentials+createRequest"></a>

### credentials.createRequest([params]) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
Creates a signed request token (JWT) given a request params object.

**Kind**: instance method of <code>[Credentials](#Credentials)</code>  
**Returns**: <code>Promise.&lt;Object, Error&gt;</code> - a promise which resolves with a signed JSON Web Token rejects with an error  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| [params] | <code>Object</code> | <code>{}</code> | request params object |

**Example**  
```js
const req = { requested: ['name', 'country'],
               callbackUrl: 'https://myserver.com',
               notifications: true }
 credentials.createRequest(req).then(jwt => {
     ...
 })

 
```
<a name="Credentials+receive"></a>

### credentials.receive(token, [callbackUrl]) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
Receive signed response token from mobile app. Verifies and parses the given response token.

**Kind**: instance method of <code>[Credentials](#Credentials)</code>  
**Returns**: <code>Promise.&lt;Object, Error&gt;</code> - a promise which resolves with a parsed response or rejects with an error.  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| token | <code>String</code> |  | a response token |
| [callbackUrl] | <code>String</code> | <code></code> | callbackUrl |

**Example**  
```js
const resToken = 'eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NksifQ.eyJyZXF1Z....'
 credentials.receive(resToken).then(res => {
     const credentials = res.verified
         const name =  res.name
     ...
 })

 
```
<a name="Credentials+push"></a>

### credentials.push(token) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
Send a push notification to a user, consumes a token which allows you to send push notifications
 and a url/uri request you want to send to the user.

**Kind**: instance method of <code>[Credentials](#Credentials)</code>  
**Returns**: <code>Promise.&lt;Object, Error&gt;</code> - a promise which resolves with successful status or rejects with an error  

| Param | Type | Description |
| --- | --- | --- |
| token | <code>String</code> | a push notification token (get a pn token by requesting push permissions in a request) |

<a name="Credentials+attest"></a>

### credentials.attest([credential]) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
Create a credential (a signed JSON Web Token)

**Kind**: instance method of <code>[Credentials](#Credentials)</code>  
**Returns**: <code>Promise.&lt;Object, Error&gt;</code> - a promise which resolves with a credential (JWT) or rejects with an error  

| Param | Type | Description |
| --- | --- | --- |
| [credential] | <code>Object</code> | a unsigned credential object |
| credential.sub | <code>String</code> | subject of credential (a uPort address) |
| credential.claim | <code>String</code> | claim about subject single key value or key mapping to object with multiple values (ie { address: {street: ..., zip: ..., country: ...}}) |
| credential.exp | <code>String</code> | time at which this claim expires and is no longer valid |

**Example**  
```js
credentials.attest({
  sub: '5A8bRWU3F7j3REx3vkJ...', // uPort address of user, likely a MNID
  exp: <future timestamp>,
  claim: { name: 'John Smith' }
 }).then( credential => {
  ...
 })
```
<a name="Credentials+lookup"></a>

### credentials.lookup(address) ⇒ <code>Promise.&lt;Object, Error&gt;</code>
Look up a profile in the registry for a given uPort address. Address must be MNID encoded.

**Kind**: instance method of <code>[Credentials](#Credentials)</code>  
**Returns**: <code>Promise.&lt;Object, Error&gt;</code> - a promise which resolves with parsed profile or rejects with an error  

| Param | Type | Description |
| --- | --- | --- |
| address | <code>String</code> | a MNID encoded address |

**Example**  
```js
credentials.lookup('5A8bRWU3F7j3REx3vkJ...').then(profile => {
    const name = profile.name
    const pubkey = profile.pubkey
    ...
  })
```
