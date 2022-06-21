## Chapter 3 Day 2 Quests by Vetal.

>1.Write your own smart contract that contains two state variables: an array of resources, and a dictionary of resources. Add functions to remove and add to each of them. They must be different from the examples above.

    pub contract ShitcoinResourceBag {

        pub var arrayOfShitcoins: @[Shitcoin]
        pub var dictionaryOfShitcoins: @{String: Shitcoin}

        pub resource Shitcoin {
            pub let ticker: String
            init() {
                self.ticker = "DOGE"
            }
        }

        pub fun arr_addCoin(coin: @Shitcoin) {
            self.arrayOfShitcoins.append(<- coin)
        }

        pub fun arr_removeCoin(index: Int): @Shitcoin {
            return <- self.arrayOfShitcoins.remove(at: index)
        }

        pub fun dic_addCoin(coin: @Shitcoin) {
            let key = coin.ticker 
            let oldCoin <- self.dictionaryOfShitcoins[key] <- coin
            destroy oldCoin
        }   

        pub fun dic_removeCoin(key: String): @Shitcoin {
            let coin <- self.dictionaryOfShitcoins.remove(key: key) ?? panic("Could not find the coin!")
            return <- coin
        }   

        init() {
            self.arrayOfShitcoins <- []
            self.dictionaryOfShitcoins <- {}
        }

    }
