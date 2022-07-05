## Chapter 5 Day 1 Quests by Vetal.

>1.Describe what an event is, and why it might be useful to a client.

Event is a way for a smart contract to communicate to the outside world that something happened. It can help clients (people reading from our contract)  know when something happens (ex. another NFT minted), and update their code accordingly. 

>2.Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened.

```cadence
pub contract ShitCoinHandlerWInterface {

  //event of changing ticker
  pub event TickerChanged(newTicker: String, prevTicker: String)

  pub resource interface ICoin {
    pub var ticker: String
  }

  pub resource ShitCoin: ICoin {
    pub var ticker: String
    pub var remark: String

    pub fun changeTicker(newTicker: String) {
      var prevTicker = self.ticker
      self.ticker = newTicker
      //broadcast event/
      emit TickerChanged(newTicker: newTicker, prevTicker: prevTicker)
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


>3.Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.

>4.For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.
