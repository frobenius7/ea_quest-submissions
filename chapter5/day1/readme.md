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
      //checks if ticker has at least 1 letter
      pre {
        newTicker.length > 0: "Ticker name is too short."
      }
      //checks if new ticker is not equal previous state of ticker
      post {
        before(self.ticker) != self.ticker : "Ticker already has this name before"
      }    
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

>4.For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.

```cadence
pub contract Test {

  // TODO
  // Tell me whether or not this function will log the name.
  // name: 'Jacob'
  pub fun numberOne(name: String) {
    pre {
      name.length == 5: "This name is not cool enough."
    }
    log(name)
  }

  // TODO
  // Tell me whether or not this function will return a value.
  // name: 'Jacob'
  pub fun numberTwo(name: String): String {
    pre {
      name.length >= 0: "You must input a valid name."
    }
    post {
      result == "Jacob Tucker"
    }
    return name.concat(" Tucker")
  }

  pub resource TestResource {
    pub var number: Int

    // TODO
    // Tell me whether or not this function will log the updated number.
    // Also, tell me the value of `self.number` after it's run.
    pub fun numberThree(): Int {
      post {
        before(self.number) == result + 1
      }
      self.number = self.number + 1
      return self.number
    }

    init() {
      self.number = 0
    }

  }

}
```
