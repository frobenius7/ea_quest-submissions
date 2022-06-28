## Chapter 4 Day 1 Quests by Vetal.

>1.Explain what lives inside of an account.

Inside account we have Contract code and Account Storage. Account storage consists of 3 parts: storage, public and private areas.

>2.What is the difference between the /storage/, /public/, and /private/ paths?

 - /storage/ - ALL account data lives here.  only the account owner can access this area.
 - /public/ - area available to everybody
 - /private/ - area only available to the account owner and the people that the account owner gives access to

>3.What does .save() do? What does .load() do? What does .borrow() do?

  - .save() function saves data to some path we specify in account storage.  
  - .load() function loads data from some path we specify in account storage.
  - .borrow() function gets a REFERENCE for data\resourse, stored in account, not the resourse itself. 

>4.Explain why we couldn't save something to our account storage inside of a script.

Because /storage/ path could be accessed only in transaction and only in "prepare" part of it. (and only by ownwer - AuthAccount)

>5.Explain why I couldn't save something to your account.

Because /storage/ can be only accessed by account signer (so when i sign transaction for myself i can access only my account storage, not someone else)

>6.Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions:
  > - A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.
  > - A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.


**contract**
```cadence
pub contract ShitCoinHandler {

  pub resource ShitCoin {
    pub var ticker: String
    init() {
      self.ticker = "DOGE"
    }
  }

  pub fun createTestCoin(): @ShitCoin {
    return <- create ShitCoin()
  }
```

**transaction1 - save, load, log and destroy resource**
```cadence
import ShitCoinHandler from 0x03

transaction {

  prepare(signer: AuthAccount) {
    let testResource <- ShitCoinHandler.createTestCoin()
    signer.save(<- testResource, to: /storage/MyTestResource) 
    let testResource2 <- signer.load<@ShitCoinHandler.ShitCoin>(from: /storage/MyTestResource)
                              ?? panic("A `@ShitCoinHandler.ShitCoin` resource does not live here.")
    log(testResource2.ticker) /// returns "DOGE"
    destroy testResource2
  }

  execute {

  }
}
```

**transaction2 - save, borrow, log resource**

```cadence
import ShitCoinHandler from 0x03

transaction {

  prepare(signer: AuthAccount) {
    let testResource <- ShitCoinHandler.createTestCoin()
    signer.save(<- testResource, to: /storage/MyTestResource) 
    let testResource2 = signer.borrow<&ShitCoinHandler.ShitCoin>(from: /storage/MyTestResource)
                          ?? panic("A `@ShitCoinHandler.ShitCoin` resource does not live here.")
    log(testResource2.ticker) // "DOGE"
  }

  execute {

  }
}
```
