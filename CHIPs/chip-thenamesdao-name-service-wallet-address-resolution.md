CHIP Number   | < Creator must leave this blank. Editor will assign a number.>
:-------------|:----
Title         | Name Service Wallet Address Resolution
Description   | A standard for NFTs to provide resolution of a name into a Chia address
Author        | Right Sexy Orc: Keybase @rightsexyorc
Editor        | < Creator must leave this blank. Editor will be assigned.>
Comments-URI  | < Creator must leave this blank. Editor will assign a URI.>
Status        | < Creator must leave this blank. Editor will assign a status.>
Category      | Informational
Sub-Category  | Guideline
Created       | 2022-10-18
Requires      | 0007
Replaces      |
Superseded-By |

## Abstract
This CHIP provides a method for an NFT to indicate a name that can map, or resolve, to a Chia blockchain address, with an optional expiry block number.

## Motivation
Chia blockchain addresses are long and hard for humans to memorize, communicate and transcribe.

Name services such as Domain Name Service (DNS) and Ethereum Name Service (ENS) are examples of methods for solving this problem for IP addresses and Ethereum blockchain addresses.

This proposal benefits the ecosystem by facilitating transaction creation, as well as Chia adoption by a larger number of humans.

Use cases for this address resolution include helping people communicate their payment address to others using their memory when they don't have their address available, and helping people transcribe their address when communicated verbally. It can also reduce error rates when copied and pasted in a computer manually, by making human verification of the transcription easier.

This proposal has already been implemented by Namesdao and our team has received confirmation by name holders of instances where it helped them communicate a payment address to others, so technical feasibility has been demonstrated and there has been some validation of use cases.

## Backwards Compatibility
No backwards incompatibilities

## Rationale
We looked to leverage existing patterns and designs when we designed this approach.

We looked especially to the Ethereum Name Service, which also uses an NFT approach.

We also looked to integrate with and leverage the existing primitives and design of the Chia blockchain.

A Chia NFT-based approach would leverage the existing infrastructure built throughout the ecosystem as well as existing primitives such as offers. Various wallets already support transfers and display of NFT's, and various NFT and blockchain explorers already support the standard as well. Additionally, various participants in the Chia community already are owners of NFT's and in many cases, have experience transferring or purchasing NFT's.

As a design decision, and to maintain required data on-chain, we decided to embed the name and expiry block into the metadata URI, and also to use DID-backed NFT's.

We considered exploring DID based possibilities, but there is very limited DID adoption beyond DID-backed NFT's, and no Accepted status CHIP specification.

One objection raised was based on the belief that name resolution required loading of offchain metadata, but that is not the case, since the data is in the NFT's URI itself. Thousands of Names have been registered through [Namesdao](https://www.namesdao.org/). The design's reference implementation has been integrated into the [chia-crypto-utils developer toolkit](https://github.com/irulast/chia-crypto-utils), which is a toolkit used by many wallet developers.


## Specification

### Primary Resolution

Names are referenced in lowercase. Primary resolution is performed exclusively using the Chia blockchain.

If there is no dot extension at the end of the name, e.g. .xch, the name will be interpreted as a .xch extension name, e.g. _namesdaotime will reference  _namesdaotime.xch.

A Wallet Address-resolving Name on the Chia blockchain is a NFT1 coin minted using the Name Service's DID. A name record for resolving the Name consists of five important values:

    1. the NFT coin id (provided per NFT1)
    2. The name, which is lowercase (required)
    3. The expiry block, an integer (optional)
    4. The Chia blockchain NFT owner address, per the NFT1 specification (provided per NFT1)
    5. Minter DID (per NFT1)

A. To extract the Name and Expiry Block from a Name NFT:

    1. Get the second-to-last metadata URI.
    2. Remove the .json ending
    3. Separate the text following the last "/" and split by the last "-" into two fields.
    4. URL-decode the first field to obtain the name.
    5. The (optional) expiry block is the second field.

B. The Wallet address for the name is the NFT owner address, per the NFT1 specification (CHIP-0005).

Full node and blockchain explorers should resolve names directly from the blockchain using this technique.

### Secondary Resolution Services

Name Services or third-parties may provide secondary name resolution services that may help an application avoid the need to index all names on the blockchain or have a full copy of the blockchain.

Two techniques can be used for verifying secondary name resolver data, depending on the application.

  1. The secondary resolver may supply the NFT coin id, so that a full node wallet can look up the NFT coin quickly and verify from its own copy of the blockchain without having to index all names locally. This uses the secondary resolver in a trustless way, though it might not resolve all registered names if not all have been added correctly to the secondary resolver.

  2. The secondary resolver may return data that is PGP signed with a public key of the name service or third party secondary resolution service provider. This could be useful for a light wallet, and avoid the risk of man-in-the-middle attacks and domain seizures.


## Test Cases
  * Test resolving unregistered name
  * Test resolving expired name
  * Test resolving recently transferred name
  * Test resolving newly registered name


## Reference Implementation
Implemented by [Namesdao](https://www.namesdao.org/)

We will provide a code sample for parsing the name and expiry block from a URI value.

Additional information is available in [Namesdao's NDIP-0002](https://github.com/theNamesdao/ndips/blob/main/ndips/ndip-0002.md).

## Security

A compromise of the Namespace's DID would let an attacker issue new Names. Each Name Service must take measures to secure its DID, as must any DID-backed NFT minter.


## Additional Assets
N/A

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
