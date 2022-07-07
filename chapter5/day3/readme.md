## Chapter 5 Day 3 Quests by Vetal.

>1.What does "force casting" with as! do? Why is it useful in our Collection?

"force cast" operator "downcasts" our generic type (@NonFungibleToken.NFT) to be a more specific type (@NFT). 
Now we can be sure that we can only deposit NFT only specific type into our Collection.

>2.What does auth do? When do we use it?

`auth` keyword helps us to get "authorized feference". 
When we are using references, if we want to downcast, we must have an `auth` reference.


>3.This last quest will be your most difficult yet. Take this contract:

original contract
```cadence
import NonFungibleToken from 0x02
pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return (&self.ownedNFTs[id] as &NonFungibleToken.NFT?)!
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```

>and add a function called borrowAuthNFT just like we did in the section called "The Problem" above. Then, find a way to make it publically accessible to other people so they can read our NFT's metadata. Then, run a script to display the NFTs metadata for a certain id.
You will have to write all the transactions to set up the accounts, mint the NFTs, and then the scripts to read the NFT's metadata. We have done most of this in the chapters up to this point, so you can look for help there :)

solution: after we have added `borrowAuthNFT` function, we need to declare interface `MyCollectionPublic` with this function and point our collection to this interface.

solution contract:

```cadence
import NonFungibleToken from 0x02

pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub let CollectionPublicPath: PublicPath
  pub let CollectionStoragePath: StoragePath  

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

//new interface with borrowAuthNFT function  
  pub resource interface MyCollectionPublic {
    pub fun deposit(token: @NonFungibleToken.NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64) : &NonFungibleToken.NFT
    pub fun borrowAuthNFT(id: UInt64) : &NFT
  }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, MyCollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return (&self.ownedNFTs[id] as &NonFungibleToken.NFT?)!
    }

    pub fun borrowAuthNFT(id: UInt64): &NFT {
        let ref = (&self.ownedNFTs[id] as auth &NonFungibleToken.NFT?)!
        return ref as! &NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    self.CollectionPublicPath = /public/CPCollection
    self.CollectionStoragePath = /storage/CPCollection 
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```

Script to read metadata 
```cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

// This script borrows an NFT from a collection
pub fun main(address: Address, id: UInt64): &CryptoPoops.NFT {
    let account = getAccount(address)
    let collectionRef = getAccount(address).getCapability(/public/CPCollection)
        .borrow<&CryptoPoops.Collection{CryptoPoops.MyCollectionPublic}>()
        ?? panic("Could not borrow capability from public collection")

    let nft = collectionRef.borrowAuthNFT(id: id) 

    log(nft.name)

    return nft
}
```

Transaction - Init acc
```cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

transaction {

    prepare(signer: AuthAccount) {
        signer.save(<- CryptoPoops.createEmptyCollection(), to: /storage/CPCollection)

        signer.link<&CryptoPoops.Collection{CryptoPoops.MyCollectionPublic, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic}>
            (CryptoPoops.CollectionPublicPath, target: CryptoPoops.CollectionStoragePath)
    }
}
```

transaction - mint NFT
```cadence
import CryptoPoops from 0x01

transaction(name: String, food: String, number: Int, recipient: Address) {

    let minter: &CryptoPoops.Minter

    let recipientCollectionRef: &CryptoPoops.Collection{CryptoPoops.MyCollectionPublic}

    prepare(signer: AuthAccount) {

        self.minter = signer.borrow<&CryptoPoops.Minter>(from: /storage/Minter)
            ?? panic("Account does not store an object at the specified path")

        // Borrow the recipient's public NFT collection reference
        self.recipientCollectionRef = getAccount(recipient)
            .getCapability(CryptoPoops.CollectionPublicPath)
            .borrow<&CryptoPoops.Collection{CryptoPoops.MyCollectionPublic}>()
            ?? panic("Could not get receiver reference to the NFT Collection")
    }

    execute {
        // Mint the NFT and deposit it to the recipient's collection
        let nft <- self.minter.createNFT(name: name, favouriteFood: food, luckyNumber: number)
        self.recipientCollectionRef.deposit(token: <- nft)
    }
}
```
