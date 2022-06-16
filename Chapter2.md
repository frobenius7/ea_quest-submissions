# quest-submissions by Vetal
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


