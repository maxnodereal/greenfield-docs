# Best Practice for Storing NFT Data using Greenfield

## Overview

BNB Greenfield is an innovative blockchain and storage platform that seeks to unleash the power of decentralized technology on data ownership and the data economy.
Greenfield stands out from existing centralized and decentralized storage systems and is best suitable for NFT storage due to its critical designs:

- Decentralized storage which enables user to access anywhere anytime
- NFT metadata is mutable on Greenfield without changing any data on the NFT smart contract, while Arweave and IPFS is immutable.
- Competitive price( check [price calculator ](https://dcellar.io/pricing-calculator))

## NFT Storage on Greenfield

Most NFTs will need some kind of structured metadata to describe the token's essential properties. Many encodings and data formats can be used, but the de-facto standard is to store metadata as a JSON object.

Here's an example of some JSON metadata for an NFT:

```json
{

 "name": "Tenset Zealy Reward #99",

 "description": "This Tenset NFT is a token of appreciation for your outstanding contribution to the Tenset community. You've earned your place in the top 100, and this NFT is a testament to your dedication.",

 "image": "https://greenfield-sp.bnbchain.org/view/zealy-rewards/zealy-reward.jpg"

}
```
There are many ways to structure metadata for an NFT. The example above uses the schema defined in the [ERC-721 ](https://eips.ethereum.org/EIPS/eip-721)standard.

### Store NFT Image on Greenfield

Storing the NFT media on Greenfield is better than storing an HTTP gateway URL, since it's not tied to a specific gateway provider.

Once NFT media are uploaded on Greenfield, it will generate an unique URL for each media, which can be put into NFT metadata.

Here's an example of media Stored on Greenfield:

https://greenfield-sp.bnbchain.org/view/gnfd-bucket/999.png

### Store Metadata Json on Greenfield

It's suggested to store NFT Metadata Json files on Greenfield too. Here's an example of a NFT metadata stored on Greenfield:

https://greenfield-sp.bnbchain.org/view/zealy-rewards/99.json

## NFT Minting using Greenfield

Greenfield Decentralized Storage Network is naturally suited for NFT Storage. To mint NFT on Greenfield, usually it takes 3 steps:

1. Get BNB on Greenfield
2. Upload both NFT metadata and media to Greenfield
3. Mint NFT

### Get BNB on Greenfield

[Greenfield Bridge](https://greenfield.bnbchain.org/en/bridge) or [DCellar ](https://dcellar.io/) can be used to transfer BNB token from BSC to Greenfield, this will automatically create a Greenfield account which shares the same account address as BSC account.

### Upload both NFT metadata and media to Greenfield

For NFT collections of less than 100 files, DCellar WebUI supports uploading multiple files.

Usually, NFT collections might contain thousands NFTs at a time, [gnfd-cmd](https://docs.bnbchain.org/greenfield-docs/docs/tutorials/cli/file-management/overview) can be a better tool to upload multiple files at one time.

To begin, follow Greenfield documentation to[ Set up Environment](https://docs.bnbchain.org/greenfield-docs/docs/tutorials/cli/file-management/overview#setting-up-the-environment) and [ Import Account and Generating Keystore](https://docs.bnbchain.org/greenfield-docs/docs/tutorials/cli/file-management/overview#impport-account-and-generating-keystore)

**Create a bucket for Your NFT Collection**

It’s suggested that you put all your NFT metadata json files within one bucket. A bucket is a logical container for storing files in Greenfield. Naming your bucket using your NFT collection name is a good choice.

To create a bucket one needs to call the following storage make-bucket command with the desired bucket name.

./gnfd-cmd bucket create gnfd://nft-collection-01

The operation will automatically choose a storage provider and submit a transaction to BNB Greenfield blockchain to write the associated metadata. Alternatively you can provide a storage provider address, that is operator-address, if a specific provider should be used. The result should look something similar to the following:

choose primary sp: https://greenfield-sp.bnbchain.org:443

create bucket nft-collection-01 succ, txn hash: 0x6c89316c5912cda8b69eac6e96aa644d374c39c635e07fae4297e03496e7726a

As you can see, the result returns a transaction hash, which one can inspect using the block explorer, e.g. [https://greenfieldscan.com](https://greenfieldscan.com/)

**Use an existing bucket for Your NFT Collection**

If there is an existing bucket which is available for use, the bucket creation step can be skipped.

**Upload NFT Images/ Metadata Json**

Prepare your NFT images on your device before you start to upload. Let's say that there is a folder named “nft-collection-01-image” where you store all your NFT images. Images are suggested to be named after their NFT token id.

Here is a example of image naming:

$ ls

001.jpg 002.jpg 003.jpg

To upload all these files into your bucket , you can run the following commands:

gnfd-cmd object put --recursive ./nft-collection-01-image gnfd://nft-collection-01

Please note that --recursive is used to put all files or objects under the specified directory or prefix in a recursive way. The default value is false

The successful result should be similar to the following:

sealing...

upload 001.jpg to gnfd://nft-collection-01

sealing...

upload 002.jpg to gnfd://nft-collection-01

sealing...

upload 003.jpg to gnfd://nft-collection-01

You can go to [GreenfieldScan](https://testnet.greenfieldscan.com/) to view the change of your bucket.

All your NFT images will be uploaded as public files, and you can get their universal endpoint under a fixed format.

[https://gnfd-testnet-sp-1.bnbchain.org/view/nft-collection-01/001.jpg](https://gnfd-testnet-sp-1.bnbchain.org/download/nft-collection/001.jpg).

[https://gnfd-testnet-sp-1.bnbchain.org/view/nft-collection-01/002.jpg](https://gnfd-testnet-sp-1.bnbchain.org/download/nft-collection/002.jpg).

[https://gnfd-testnet-sp-1.bnbchain.org/view/nft-collection-01/003.jpg](https://gnfd-testnet-sp-1.bnbchain.org/download/nft-collection/003.jpg).

Once you get your image URLs, you can use them to construct your NFT metadata.

Here is an example of NFT metadata.json

{

 "name": "nft-collection-01 #001",

 "description": "This NFT is an example of nft minting using Greenfield",

 "image": "[https://gnfd-testnet-sp-1.bnbchain.org/download/nft-collection-01/001.jpg](https://gnfd-testnet-sp-1.bnbchain.org/download/nft-collection/001.jpg)"

}

We suggest you name your NFT metadata json using NFT token id, such as 01.json.

And you can upload all these files into your bucket , you can run the following commands:

gnfd-cmd object put --recursive ./nft-collection-01-json gnfd://nft-collection-01

**Highlight**:

Please make sure that the payment account of your bucket has not only enough balance to store all the images and metadata json files in a very long time, but also the read/download quota. you can always check your account balance in GreenfieldScan/DCellar. Also DCellar is a good tool to buy /mange quota

### Mint NFT

Now that your NFT images/metadata json files are stored with Greenfield, you can mint tokens using the blockchain platform of your choice.

## NFT Migration

Before you start to conduct NFT migrate, make sure the NFT contract is capable of updating the base URI after minting .

For example, if your NFT contract is ERC721 and you can find a method called set base url in your contract, then it indicates that the NFT base URI can be updated.

Only if the NFT contract is capable of updating the base URI after minting , you can migrate your NFT images/metadata json files to Greenfield.

If you have already minted your NFT and stored your NFT images somewhere, and your NFT is mutable. Now you want to migrate your NFT images to Greenfield, we provide a simple example to help you understand how to migrate your NFT images to Greenfield :

https://github.com/bnb-chain/greenfield-nft-migration

In this example, we assume token ids are continuous, and metadata contains name& image, and image URL is using HTTP protocol. For example,

{

 "name": "",

 "description": "",

 "image": ""

}

We can breakdown NFT Migration into 4 steps:

1. Setup Environment
2. Migrate NFT images to Greenfield
3. Update NFT

### Setup Environment

Please refer to https://github.com/bnb-chain/greenfield-nft-migration#readme to Setup the environment to run the example.

### Migrate NFT images to Greenfield

Get your NFT contact , we take *0xA5FDb0822bf82De3315f1766574547115E99016f* as an example. It's an ERC721 contract deployed on BSC Testnet.

Run

python3 migrate-nft.py --contract=0xA5FDb0822bf82De3315f1766574547115E99016f

By running the example, the script will extract NFT image URL after parsing the contract, and then invoke gnfd-cmd to create a bucket and upload those NFt images to Greenfield.And it will print the image urls generated on Greenfield which can be use to update NFTs.



download json file and upload images



update and upload json file


### Update NFT

Now that NFT images are stored on Greenfield now, the next step should be update NFT image urls through calling smart contract.

We won't attempt to illustrate the NFT update process here, because the details depend on which chain and development language you're using, as well as the contract and standards you're targeting.