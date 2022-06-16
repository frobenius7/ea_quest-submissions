# quest-submissions (Chapter 2) by Vetal
#
## Chapter 2 Day 1 Quests.

Tasks

>Deploy a contract to account 0x03 called "JacobTucker". Inside that contract, declare a constant variable named is, and make it have type String. Initialize it to "the best" when your contract gets deployed.

>Check that your variable is actually equals "the best" by executing a script to read that variable. Include a screenshot of the output.

### contract

    pub contract JacobTucker {

        pub let is: String

        init() {
            self.is = "the best"
        }
    }

### script

      import JacobTucker from 0x03

      pub fun main(): String {
          return JacobTucker.is
      }

### output screen

<img width="707" alt="image" src="https://user-images.githubusercontent.com/7878433/174072033-842315b1-39d3-45a2-8394-3574c484588a.png">

## Chapter 2 Day 2 Quests.

Please answer in the language of your choice.

>1.Explain why we wouldn't call changeGreeting in a script.
>
Because our mission is to change data in blockchain (script only views data)

>2.What does the AuthAccount mean in the prepare phase of the transaction?
>
It helps us to access data, stored on our account (who signs the transaction)

>3.What is the difference between the prepare phase and the execute phase in the transaction?
>
In prepare phase we are acessing data, stored on the account. in the execute phase we change data on blockchain (smart contract)

>4.This is the hardest quest so far, so if it takes you some time, do not worry! I can help you in the Discord if you have questions.
>Add two new things inside your contract:
> - A variable named myNumber that has type Int (set it to 0 when the contract is deployed)
> - A function named updateMyNumber that takes in a new number named newNumber as a parameter that has type Int and updates myNumber to be newNumber
>Add a script that reads myNumber from the contract

>Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function. Verify that your number changed by running the script again.

### contract
      pub contract JacobTucker {

          pub let is: String
          pub var myNumber: Int

          init() {
              self.is = "the best"
              self.myNumber = 0
          }

          pub fun updateMyNumber(newNumber: Int) {
              self.myNumber = newNumber
          }
      }

### script

      import JacobTucker from 0x03

      pub fun main(): Int {
          return JacobTucker.myNumber
      }
      
### transaction

      import JacobTucker from 0x03

      transaction (txNumber: Int) {
        prepare(signer: AuthAccount) {
          log(signer.address)
        }

        execute {
            JacobTucker.updateMyNumber(newNumber: txNumber)
        }
      }


### output screen

<img width="1164" alt="image" src="https://user-images.githubusercontent.com/7878433/174079847-a7d8b659-fc2b-4478-a117-1d1b6c7457be.png">

<img width="431" alt="image" src="https://user-images.githubusercontent.com/7878433/174079936-62aef48b-9c1f-4894-b3e9-80c9151d00bb.png">


## Chapter 2 Day 3 Quests.

>1.In a script, initialize an array (that has length == 3) of your favourite people, represented as Strings, and log it.

        pub fun main() {
            var myFavouritePeople: [String] = ["Steve Jobs", "Elon Musk", "Jacob Tucker"]
            log(myFavouritePeople)
        }
        
![image](https://user-images.githubusercontent.com/7878433/174138505-160e0c09-ef9e-4ede-aff0-004f51a0ecdc.png)

>2.In a script, initialize a dictionary that maps the Strings Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a UInt64 that represents the order in which you use them from most to least. For example, YouTube --> 1, Reddit --> 2, etc. If you've never used one before, map it to 0!

        pub fun main() {
            var socialNetworks: {String: UInt64} = {"Facebook": 4, "Instagram": 2, "Twitter": 1, "YouTube": 3, "Reddit": 5, "LinkedIn": 6}
            log(socialNetworks.keys) 
            log(socialNetworks.values)
        }

![image](https://user-images.githubusercontent.com/7878433/174138565-a6536f55-ea09-45fb-9647-996e167c31a2.png)

>3.Explain what the force unwrap operator ! does, with an example different from the one I showed you (you can just change the type).

When you access elements of a dictionary, it returns the value as an optional. 
![Pasted Graphic 2](https://user-images.githubusercontent.com/7878433/174138205-edbe1aa0-0367-4c43-971e-84007e55ea6c.png)

So you need to unwrap it before.
![Pasted Graphic 4](https://user-images.githubusercontent.com/7878433/174138264-1762b209-0487-4842-bd0f-3dc948daa90e.png)


>4.Using this picture below, explain...
>
![image](https://user-images.githubusercontent.com/7878433/174137672-c1c351d9-dfa0-480e-bf57-62662eb86334.png)

> - What the error message means

This message means that program expecting another type as output to main function. 

> - Why we're getting this error

on line 1 we have declared “string” output type for function main and return optional type "string?" on line 3.

> - How to fix it

there are 2 options:

    1.we can unwrap output by adding “!” on line 3. (return thing [0x03]!)
    2.we can change declared output value type in line 1 (from String to String?)
