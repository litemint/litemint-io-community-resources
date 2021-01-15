# litemint.io Shuffle Algorithm
Game URL：**[https://litemint.io](https://litemint.io)**

## Rationale

Ensure that regardless of the number of DEX card added, a player is unlikely to create a broken deck leading to deadlocks (i.e. stuck in an infinite game).

Provide players with enough freedom for depth and variety to embrace different playstyles with deck composition (Agro, Control...).

Ensure that players do not need to worry about running out of cards and card counting.

## Rest API

The endpoint to generate random decks is documented here and can be used for simulators:
https://github.com/litemint/litemint-io-community-resources/blob/master/public_rest_api.md#get-deck

## Rotation

Once a card is played it goes to the end of the deck so it is possible to predict the order in which they will be available again.

Forced draw mechanic (Victory Rush, Trojan) inserts the forced card at the top of the player deck without modifying the other card order. These forced cards will be drawn again after a full rotation.

Edge Case: Boomerang will be drawn again after a full rotation but will not trigger the forced draw effect (occurs once).

## Shuffle Pseudo code

```
Initialize BaseDamageSet with card_base_00005, card_base_00008, card_base_00010, card_base_00011
Initialize BaseHealSet with card_base_00003, card_base_00006, card_base_00009
Initialize BaseEnergySet with card_base_00001, card_base_00004, card_base_00007

Remove all passive cards from PlayerDeck

While PlayerDeck has less than 4 cards
    Add one card_base_00002 (Hellfire I) to PlayerDeck

Randomly shuffle BaseDamageSet
Randomly shuffle BaseHealSet
Randomly shuffle BaseEnergySet

Randomly shuffle PlayerDeck
Trim PlayerDeck to 10 cards (max rotation)

If player has card_dex_00060 (Brawler)
    Replace the first BaseHealSet card with card_dex_00061 (Brawl)

If player has card_dex_00031 (Primal Overload)
    Replace card_base_00001 (Overload I) with card_dex_00031 (Primal Overload) in BaseEnergySet

Randomly shuffle (BaseDamageSet[0], BaseHealSet[0], BaseEnergySet[0], PlayerDeck[0], PlayerDeck[1])
Add the result to FinalDeck

Randomly shuffle (BaseDamageSet[1], BaseDamageSet[3], BaseEnergySet[1], PlayerDeck[2])
Add the result to FinalDeck

Randomly shuffle (BaseDamageSet[2], BaseHealSet[2], BaseEnergySet[2], PlayerDeck[3])
Add the result to FinalDeck

If PlayerDeck length > 4
    Randomly shuffle (BaseDamageSet[0], BaseHealSet[1], BaseEnergySet[0], PlayerDeck[4])
    Add the result to FinalDeck

If PlayerDeck length > 5
    Randomly shuffle (BaseDamageSet[1], BaseDamageSet[3], BaseEnergySet[1], PlayerDeck[5])
    Add the result to FinalDeck

If PlayerDeck length > 6
    Randomly shuffle (BaseDamageSet[2], BaseHealSet[2], BaseEnergySet[2], PlayerDeck[6])
    Add the result to FinalDeck

If PlayerDeck length > 7
    Randomly shuffle (BaseDamageSet[0], BaseHealSet[1], BaseEnergySet[0], PlayerDeck[7])
    Add the result to FinalDeck

If PlayerDeck length > 8
    Randomly shuffle (BaseDamageSet[1], BaseDamageSet[3], BaseEnergySet[1], PlayerDeck[8])
    Add the result to FinalDeck

If PlayerDeck length > 9
    Randomly shuffle (BaseDamageSet[2], BaseHealSet[2], BaseEnergySet[2], PlayerDeck[9])
    Add the result to FinalDeck    
    
Initialize FavoriteCardSet with all player favorited cards (starred)
Randomly shuffle FavoriteCardSet

For each FavoriteCard in FavoriteCardSet
    If PlayerDeck has card_dex_00056 (Chosen One) or getRandom(0,1) <= 0.75
       Move FavoriteCard to a random position within the first 3 cards in FinalDeck
       break

return FinalDeck
```
