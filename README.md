# MinifiedRaindropSDKs
This is an unofficial ligtweigth Hydrogen Raindrop SDK for developers that wish to use Raindrop on Javascript Client.
# The need for Minified Raindrop SDK
1. The official Raindrop Javascript version mainly suit Nodejs developers
2. Converting official Raindrop SDK to client side version increases the file size to over 2MB( using Browserify), thus, it can freeze browsers when loading the file.
3. The size of the unofficail minified version is 383Kb, thus, loader faster on browser.<br />
<b> Note: This is test version, I need feedback from Hydrogen Community </b>
# Note: this documentation is taking from official Javascript Raindrop SDK https://github.com/hydrogen-dev/raindrop-sdk-js
# How to Use
Download the raindrop-community-sdk.min,js from this repo
and include thefile in your HTML document
```
<script  src="raindrop-community-sdk.min.js"></script>
```
The raindrop package exposes two objects: raindrop.client and raindrop.server. To start making API calls, you'll need to instantiate a RaindropPartner object for the module you'd like to use. The SDK will automatically fetch you an OAuth token, and set your environment.

Generic RaindropPartner Functions
constructor ``` new RaindropPartner(config) ```
To instantiate a new RaindropPartner object in the client or server modules, you must pass a config object with the following values:

config
``` environment (required) ```: Sandbox | Production to set your environment
``` clientId (required) ```: Your OAuth id for the Hydro API
``` clientSecret (required) ```: Your OAuth secret for the Hydro API
``` applicationId (required for client calls) ```: Your application id for the Hydro API
``` verbose (optional) ```: true | false turns more detailed error reporting on | off
``` RaindropPartnerObject.refreshToken() ```
Manually refreshes OAuth token.

``` RaindropPartnerObject.transactionStatus(transactionHash) ```
This function returns true when the transaction referenced by transactionHash has been included in a block on the Ethereum blockchain (Rinkeby if the environment is Sandbox, Mainnet if the environment is Production).

``` transactionHash (required) ```: Hash of a transaction
Generic raindrop.client Functions
``` generateMessage() ```
Generates a random 6-digit string of integers for users to sign. Uses system-level CSPRNG.

raindrop.client.RaindropPartner Functions
Client-side Raindrop initialization code will look like:
```
// Client-side Raindrop Setup
const ClientRaindropPartner = new raindrop.client.RaindropPartner({
    environment: "Sandbox",
    clientId: "yourId",
    clientSecret: "yourSecret",
    applicationId: "yourApplicationId"
})
```
```
registerUser(HydroID)
```
Should be called when a user elects to use Raindrop Client for the first time with your application.

``` HydroID ```: the new user's HydroID (the one they used when signing up for Hydro mobile app)
```
verifySignature(HydroID, message)
```
Should be called each time you need to verify whether a user has signed a message.

``` HydroID ```: the HydroID of the user that is meant to have signed message
message: a message generated from ``` generateMessage() (or any 6-digit numeric code) ```
Returns a response object that looks like: ``` {verified: true, data: {...}} ```. The verified parameter will only be true for successful verification attempts.
```
unregisterUser(HydroID)
```
Should be called when a user disables Client-side Raindrop with your application.

``` HydroID ```: the user's HydroID (the one they used when signing up for Hydro mobile app)
``` raindrop.server.RaindropPartner ``` Functions
Server-side Raindrop initialization code will look like:
```
// Server-side Raindrop Setup
const ServerRaindropPartner = new raindrop.server.RaindropPartner({
  environment: "Sandbox",
  clientId: "yourId",
  clientSecret: "yourSecret"
})
```
```
whitelist(addressToWhitelist)
```
A one-time call that whitelists a user to authenticate with your API via Server-side Raindrop.

``` addressToWhitelist ```: The Ethereum address of the user you're whitelisting
'''
requestChallenge(hydroAddressId)
...
Initiate an authentication attempt on behalf of the user associated with hydroAddressId.

``` hydroAddressId ```: the hydro_address_id of the authenticating user
``` authenticate(hydroAddressId) ```
Checks whether the user correctly performed the raindrop.

``` hydroAddressId ```: the hydro_address_id of the user who claims to be authenticated
Returns a response object that looks like: ``` {authenticated: true, data: {...}} ```. The authenticated parameter will only be true for successful authentication attempts.
