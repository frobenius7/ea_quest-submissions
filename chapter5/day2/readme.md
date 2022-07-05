## Chapter 5 Day 2 Quests by Vetal.

>1.Explain why standards can be beneficial to the Flow ecosystem.

Standardizing is beneficial so that a client using multiple contracts can have a singular way of interacting with all of those contracts. For example, all NFT contracts have a resource called Collection that has a deposit and withdraw function. This way, even if the client DApp is interacting with 100 NFT contracts, it only has to import the NonFungibleToken standard to call those functions, since it's all under one generic type.

>2.What is YOUR favourite food?

My favourite food is Pasta Carbonara and Philadelphia roll =)

>3.Please fix this code (Hint: There are two things wrong):

The contract interface:

```cadence
pub contract interface ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    pre {
      newNumber >= 0: "We don't like negative numbers for some reason. We're mean."
    }
    post {
      self.number == newNumber: "Didn't update the number to be the new number."
    }
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  pub resource Stuff {
    pub var favouriteActivity: String
  }
}
```

The implementing contract:

```cadence
pub contract Test {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    self.number = 5
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  pub resource Stuff: IStuff {
    pub var favouriteActivity: String

    init() {
      self.favouriteActivity = "Playing League of Legends."
    }
  }

  init() {
    self.number = 0
  }
}
```
