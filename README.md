# Bitski

## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first.

## Requirements

- Currently supports iOS 11 only

## Installation

Bitski is available through [CocoaPods](https://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'Bitski'
```

## Usage

First, request a client ID by signing up here: https://developer.bitski.com. Make sure you request an offline scope so that your access tokens can be refreshed.

Then in your app, you'll initialize an instance of Bitski:

```swift
// Replace redirect URL with an url scheme that will hit your native app
Bitski.shared = Bitski(clientID: "<YOUR CLIENT ID>", redirectURL: URL(string: "exampleapp://application/callback")!)
```
Once you have an instance of Bitski, you can check the signed in status. The user will need to be logged in before making any Web3 calls.

```swift
if Bitski.shared?.isLoggedIn == true {
    self.web3 = bitski.getWeb3(network: .kovan)
    // show logged in state
} else {
    // show logged out state
}
```

To sign in (this will open a browser window):

```swift
Bitski.shared?.signIn(viewController: self) { accessToken, error in
    // Currently we only support kovan and rinkeby. mainnet coming soon.
    self.web3 = Bitski.shared?.getWeb3(network: .kovan)
}
```

A user will remain signed in indefinitely, unless the access token is revoked. To explicitly sign out:

```swift
Bitski.shared?.signOut()
```

Once you have an instance of Web3 intialized, you can use it to make Ethereum calls and transactions.

```swift
firstly {
    web3.eth.accounts()
}.firstValue { accounts in
    accounts
}.then { account in
    let transaction = BitskiTransaction(to: EthereumAddress(hex: "SOME ADDRESS", eip55: false), from: account, value: 0, gasLimit: 20000)
    return web3.eth.sendTransaction(transaction: transaction)
}.done { transactionHash in
    print("Received transaction hash!", transactionHash.hex())
}
```

For more about what you can do in Web3, see [Web3.swift](https://github.com/Boilertalk/Web3.swift).

## License

Bitski is available under the MIT license. See the LICENSE file for more info.
