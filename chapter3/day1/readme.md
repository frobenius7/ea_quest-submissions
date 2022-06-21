## Chapter 3 Day 1 Quests by Vetal.

>1.In words, list 3 reasons why structs are different from resources.

1)Structs can be copied

2)Structs can be created outside of the contract. 

3)Structs are less secure (can be easyly moved, overwritten, deleted)

>2.Describe a situation where a resource might be better to use than a struct.

Resourse might be a better used when we have important and uniquie peace of data (for example NFT) and we don't want to loose it by accident.

>3.What is the keyword to make a new resource?

We create a new resouse type with the `create` keyword

>4.Can a resource be created in a script or transaction (assuming there isn't a public function to create one)?

no, we can create resourcse only inside the contract.

>5.What is the type of the resource below?

      pub resource Jacob {

      }

This is empty resourse type (it doesn't conatin any data inside.

>6 .Let's play the "I Spy" game from when we were kids. I Spy 4 things wrong with this code. Please fix them.

**original version **

      pub contract Test {

          // Hint: There's nothing wrong here ;)
          pub resource Jacob {
              pub let rocks: Bool
              init() {
                  self.rocks = true
              }
          }

          pub fun createJacob(): Jacob { // there is 1 here
              let myJacob = Jacob() // there are 2 here
              return myJacob // there is 1 here
          }
      }
      
**Fixed version **      
      
      pub contract Test {

      // Hint: There's nothing wrong here ;)
      pub resource Jacob {
          pub let rocks: Bool
          init() {
              self.rocks = true
          }
      }

      pub fun createJacob(): @Jacob { // there is 1 here
          let myJacob <- create Jacob() // there are 2 here
          return <- myJacob // there is 1 here
      }
  }
