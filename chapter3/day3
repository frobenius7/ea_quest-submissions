## Chapter 3 Day 3 Quests by Vetal.

>1.Define your own contract that stores a dictionary of resources. Add a function to get a reference to one of the resources in the dictionary.

      pub contract ShitcoinResourceBagWithRef {

          pub var dictionaryOfShitcoins: @{String: Shitcoin}

          pub resource Shitcoin {
              pub let ticker: String
              init(_ticker: String) {
                  self.ticker = _ticker
              }
          }

          pub fun getReference(key: String): &Shitcoin {
              return (&self.dictionaryOfShitcoins[key] as &Shitcoin?)!
          }

          init() {
              self.dictionaryOfShitcoins <- {
                  "MOON": <- create Shitcoin(_ticker: "DOGE"),
                  "REKT": <- create Shitcoin(_ticker: "BTC")
              }

          }
      }

>2.Create a script that reads information from that resource using the reference from the function you defined in part 1.

      import ShitcoinResourceBagWithRef from 0x04

      pub fun main(): String {
        let ref = ShitcoinResourceBagWithRef.getReference(key: "REKT")
        return ref.ticker // Result {"type":"String","value":"BTC"}
      }

>3.Explain, in your own words, why references can be useful in Cadence.

References makes our life a lot easier when we dealing with resourses (so we don't have to move them each time when we want to read any parameter or etc.)
