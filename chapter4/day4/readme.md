## Chapter 4 Day 4 Quests by Vetal.

>Take our NFT contract so far and add comments to every single resource or function explaining what it's doing in your own words.

```cadence
pub contract CryptoPoops {
  pub var totalSupply: UInt64

  // This is an NFT resource that contains a name,
  // favouriteFood, and luckyNumber
  pub resource NFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    //initialize of NFT resource 
    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  // This is a resource interface that allows us to deposit NFT token in collection,
  //get IDs of owned tokens and borrow nft from collection 
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64): &NFT
  }
  
  //this is resource that contains our collection of NFTs (resource type NFT)
  pub resource Collection: CollectionPublic {
    pub var ownedNFTs: @{UInt64: NFT}

    //deposit NFT in our collection
    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }

    //withdraw NFT from our collection
    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }

    //Get IDs what we have in our collection
    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    //this function helps us to read NFT
    pub fun borrowNFT(id: UInt64): &NFT {
      return (&self.ownedNFTs[id] as &NFT?)!
    }

    //initialize of collection
    init() {
      self.ownedNFTs <- {}
    }
    
    //destructor - need to declare destructor function of collection 
    destroy() {
      destroy self.ownedNFTs
    }
  }
  
  //create emtpty collection of resources
  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }

  //Minter resource - allows anyone who holds it to mint NFT
  pub resource Minter {

    //mints a new NFT resource
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    //creates minter resource
    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }
  
  //initialization of minter resource
  init() {
    self.totalSupply = 0
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```
