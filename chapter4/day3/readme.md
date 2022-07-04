## Chapter 4 Day 3 Quests by Vetal.

>1.Why did we add a Collection to this contract? List the two main reasons.

1st reason - if we want to have more than one nft we need to rembember and create unique storage path for every nft - it is inefficient. 

2nd reason - because account owner can store NFT in their account storagey directly, no one can mint us as NFT.. 

so we create collection container that wraps all our collection and we can allow others deposit into this collection under one storage path.

>2.What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")

We have to declare `destroy` function that manually destroys "nested" resources with the `destroy` keyword

>3.Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.
> - Idea #1: Do we really want everyone to be able to mint an NFT? 🤔.
> - Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?

We don't want anyone to be able to mint a NFT (so we can do a whitelist)

If we want to read information about NFT we can create reference to this NFT and also declare a interface, so we don't have to move our NFT out a collection and also restrict so only particular fields will be readable.
