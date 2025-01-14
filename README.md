# Solita Swift [![Swift](https://github.com/metaplex-foundation/solita-swift/actions/workflows/swift.yml/badge.svg)](https://github.com/metaplex-foundation/solita-swift/actions/workflows/swift.yml)

**Sol** ana **I** DL **t** o **A** PI generator.

## How does it Work?

_Solita_ generates a Swift SDK for your _Solana_ Rust programs from the [IDL](https://en.wikipedia.org/wiki/Interface_description_language) extracted by
[anchor](https://github.com/project-serum/anchor).

It supports Anchor/Shank IDls and will output a SPM package generated. It includes Accounts, Errors, Instructions and Types. It uses a a default borsch serializer called `beet` also included in the package.

Its a carbon copy from [Solita](https://github.com/metaplex-foundation/solita) with lots of language specific considerations. IDL processing and architecture has been maintained.

## Usage

Run solita cli against the idl.json generated by shank or solita

```sh
solita render <idlPath> <outputDir>
```
For example 

```sh
solita render Tests/SolitaTests/Resources/action_house.json .output
```

This will generate a swift package that contains a swift package of a library target. By default the target is called `Generated`. The package will contain the necessary dependencies to run the library that can be embedded into any project. Inside you will also get all the `Sources` of this generated code for this idl. 

To change the package name please provide the package name option `-p <packageName>`

```sh
solita render Tests/SolitaTests/Resources/action_house.json .output -p ActionHouse
```

Inside the `Sources` you will also get a `.swiftlint` that have the following disabled_rules. 

```yaml
disabled_rules:
 - identifier_name
 - force_cast
```

Currently there is no way to change the name scheme for the identifiers. Casting is also necessary since the parsing requires it.

Please verify your `Program.swift` and check if is pointing to the default onchain program. If the publicKey is provided by the IDL it will be shown here. If is not, please provided it or it will throw an exception during runtime.

```swift
import Foundation
import Solana

/**
* Program address
*
* @category constants
* @category generated
*/

let PROGRAM_ADDRESS = "<publickey>"

/**
* Program public key
*
* @category constants
* @category generated
*/

public let PROGRAM_ID = PublicKey(string: PROGRAM_ADDRESS)
```
## Options

| Option                                 | Short     | Long                                      | Value          |
|----------------------------------------|-----------|-------------------------------------------|----------------|
| Project Name                           | -p        | --projectName                             | String         |
| AccountsHaveImplicitDiscriminator      | -a        | --accountsHaveImplicitDiscriminator       | True or False  |

## Build from Source

Run the following command on shell to compile 

```sh
swift build -c release
```

the output binary file will be here `.build/release/SolitaCLI`

### Errors

If you receive the following error:

```sh
error: 'solita-swift': Invalid manifest
.../solita-swift/Package.swift:2:8: error: no such module 'PackageDescription'
```

Running the following command should resolve the issue:

```sh
sudo xcode-select --reset
```

## LICENSE

Apache-2.0
