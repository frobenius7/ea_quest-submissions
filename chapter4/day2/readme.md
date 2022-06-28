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

    pub fun changeTicker(newTicker: String) {
      self.ticker = newTicker
    }

    init() {
      self.ticker = "DOGE"
    }
  }

  pub fun createTestCoin(): @ShitCoin {
    return <- create ShitCoin()
  }

}
```
>Then, do the following:
> - In a transaction, save the resource to storage and link it to the public with the restrictive interface.
> - Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.
> - Run the script and access something you CAN read from. Return it from the script.
