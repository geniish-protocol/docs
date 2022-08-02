# White paper

# Abstract

# Introduction

# Problem

## Basics for EVM compatible blockchains

## How non-fungible tokens work

Technically speaking a non-fungible token or short NFT is just a reference to mappings stored on a blockchain. These mappings are interpreted as properties of that NFT. For example, the owner of the NFT with a specific tokenID is implemented as shown below.

```
mapping(uint256 => address) private _owners;
```

Where mapping references a specific data structure in the solidity language. It is comparable to a hashmap in other languages. In the configuration above, the key is of datatype uint256 and stores an address. Which in that case represents the owning wallet or contract.

So if Bob receives an NFT with tokenID 24 from Alice this mapping would contain Bob's wallet address at the key 24. All other common properties are implemented in the exact same way. So when referencing the tokenURI implementation looks at a mapping called \_tokenURIs and returns the value stored at tokenID , optionally concatenated with a base URI. By design, all these properties are public knowledge and easily accessible.

As space on-chain is limited and very expensive the tokenURI property is used to add metadata to NFTs. This is done by saving the URI to the metadata location on-chain. NFT viewers and marketplaces then fetch and interpret these metadata according to some common standards.
There are several used standards on how the metadata is formatted. The most common is the interface provided by the creators of ERC721. The snipped below shows this format.

```json
{
  "title": "Asset Metadata",
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "description": "Identifies the asset to which this NFT represents"
    },
    "description": {
      "type": "string",
      "description": "Describes the asset to which this NFT represents"
    },
    "image": {
      "type": "string",
      "description": "A URI pointing to a resource with mime type image/* representing the asset to which this NFT represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
    }
  }
}
```

The metadata itself is also common knowledge as otherwise only the information stored on-chain could be displayed for an NFT. Due to this design and the nature of blockchain, all data is transparent and readable by every participant.

## Using NFTs for exclusive content

This is a good thing. Everybody should agree on who owns what NFT and where that NFTs metadata is stored. But there are valid use cases where one wants to hide or restrict access to some part of an NFT.

A very clear use case is found in digital art. As an artist, I want to sell my art in form of an NFT to an interested party. For a party to be interested in buying the NFT he needs exclusivity. Normal NFTs only offer the exclusivity of being the sole owner of that art piece. But the art itself is public knowledge and viewable by everyone. As an artist, I should have the possibility to give NFT collectors further exclusivity. An option would be to provide a high-resolution version of my art and only feature a preview in the NFT itself.

Current structures allow this only in a very limited and centralised way. A dApp could verify the owner by a signature and then bring him to an exclusive space to show the hidden content of the NFT. But takes the trust off-chain and eliminates all advantages gained from the blockchain.

# Solution

We established that there is a need for NFTs that contain exclusive content. Current solutions solve this in a centeralized way. With Oasis Parcel we can offer a better solution that is decentralized and very elegantly. Our protocol improves this further by developing dApps and quality of live tools around that idea.

- developing a standard for NFTs with content stored on Parcel (private NFTs)
- developing a protocol using that standard
- geniish dApp
  - view, create, edit, and delete private NFTs
  - marketplace to sell and buy private NFTs
  - offer custodial access to Parcel
- creating bridges from Emerald to other chains to use private NFTs in other ecosystems
- publishing all tooling, knowledge and dApps used in that process

## Architectural specification

The diagram below is a high-level overview of the geniish protocol.

![](/assets/images/geniish_protocol.drawio.png)

## Technical specifications

### Confidential NFTs

Confidential NFTs are tokens that support either the ERC721 or ERC1155 standard, live on the Emerald chain and include a mechanism to store a reference to a Parcel token called `ParcelID`. That reference can be stored within the metadata of the token or via our ERC721 or ERC1155 extension on-chain. Parcel also tracks the reference to the private NFT via the transferability parameter.

A Parcel token can have documents attached to it. A document is just binary data stored by Parcel and only readable by a defined set of Identities. All documents for a private NFT will have its owner set to `escrow` as this only allows the owner of the Parcel token to access that document.

The diagram below is a visual representation of said references.

![](/assets/images/geniish_private_nft.drawio.png)

When a private NFT is minted. Its twin on Parcel is minted as well.

The private NFT two states, it is either locked (open) or unlocked (closed).

![Opening a private NFT to access its content](/assets/images/geniish_locking.png)

![Opening a private NFT to access its content](/assets/images/geniish_opening.png)

When locking the Emerald token on the Parcel bridge adapter, the bridge sends the Parcel token to the linked Oasis identity of the Emerald token locker. This process is easily manageable by non-technical users via the geniish dApp.

### Bridges to Emerald

### GSN
