## Chapter 4 Day 2 Quests by Vetal.

>1.What does .link() do?

"link" function creating something called capability (i.e. "mapping" from /public/ or /private/ path to our /storage/, so data can be accessed from "outside".

>2.In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.

We need to define a new resource interface in contract code, so it will only expose some things and then when we use .link() function we restrict reference by using this interface. 


>3.Deploy a contract that contains a resource that implements a resource interface. Then, do the following:
```cadence
pub contract ShitCoinHandlerWInterface {

  pub resource interface ICoin {
    pub var ticker: String
  }

  pub resource ShitCoin: ICoin {
    pub var ticker: String
    pub var remark: String

    pub fun changeTicker(newTicker: String) {
      self.ticker = newTicker
    }

    init() {
      self.ticker = "DOGE"
      self.remark = "to the MOON"
    }
  }

  pub fun createTestCoin(): @ShitCoin {
    return <- create ShitCoin()
  }

}
```
>Then, do the following:
> - In a transaction, save the resource to storage and link it to the public with the restrictive interface.
```cadence
import ShitCoinHandlerWInterface from 0x04

transaction() {
  prepare(signer: AuthAccount) {
    // Save the resource to account storage
    signer.save(<- ShitCoinHandlerWInterface.createTestCoin(), to: /storage/MyTestResource)
    // link it to the public with the restrictive interface..
    signer.link<&ShitCoinHandlerWInterface.ShitCoin{ShitCoinHandlerWInterface.ICoin}>(/public/MyTestResource, target: /storage/MyTestResource)
  }

  execute {

  }
}
```
> - Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.
```cadence
import ShitCoinHandlerWInterface from 0x04

pub fun main(address: Address): String {
  let publicCapability: Capability<&ShitCoinHandlerWInterface.ShitCoin{ShitCoinHandlerWInterface.ICoin}> =
    getAccount(address).getCapability<&ShitCoinHandlerWInterface.ShitCoin{ShitCoinHandlerWInterface.ICoin}>(/public/MyTestResource)

  let testResource: &ShitCoinHandlerWInterface.ShitCoin{ShitCoinHandlerWInterface.ICoin} = publicCapability.borrow() ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")
  
  // This wont work because `remark` is not in `&ShitCoinHandlerWInterface.ShiCoin{ShitCoinHandlerWInterface.ICoin}`
  return testResource.remark //member of restricted type is not accessible 
}
```

> - Run the script and access something you CAN read from. Return it from the script.

```cadence
import ShitCoinHandlerWInterface from 0x04

pub fun main(address: Address): String {
  let publicCapability: Capability<&ShitCoinHandlerWInterface.ShitCoin{ShitCoinHandlerWInterface.ICoin}> =
    getAccount(address).getCapability<&ShitCoinHandlerWInterface.ShitCoin{ShitCoinHandlerWInterface.ICoin}>(/public/MyTestResource)

  let testResource: &ShitCoinHandlerWInterface.ShitCoin{ShitCoinHandlerWInterface.ICoin} = publicCapability.borrow() ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")

  // This works because `name` is in `&ShitCoinHandlerWInterface.ShiCoin{ShitCoinHandlerWInterface.ICoin}`
  return testResource.ticker 
}
```
