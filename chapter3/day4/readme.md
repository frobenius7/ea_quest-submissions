## Chapter 3 Day 4 Quests by Vetal.

>1.Explain, in your own words, the 2 things resource interfaces can be used for (we went over both in today's content)

Interfaces can be used to:
  - specify a set of requirements for something to implement
  - allow us to only expose certain things to certain people


>2.Define your own contract. Make your own resource interface and a resource that implements the interface. Create 2 functions. In the 1st function, show an example of not restricting the type of the resource and accessing its content. In the 2nd function, show an example of restricting the type of the resource and NOT being able to access its content.

мчсясм

>3.How would we fix this code?

    pub contract Stuff {

        pub struct interface ITest {
          pub var greeting: String
          pub var favouriteFruit: String
        }

        // ERROR:
        // `structure Stuff.Test does not conform 
        // to structure interface Stuff.ITest`
        pub struct Test: ITest {
          pub var greeting: String

          pub fun changeGreeting(newGreeting: String): String {
            self.greeting = newGreeting
            return self.greeting // returns the new greeting
          }

          init() {
            self.greeting = "Hello!"
          }
        }

        pub fun fixThis() {
          let test: Test{ITest} = Test()
          let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
          log(newGreeting)
        }
    }
