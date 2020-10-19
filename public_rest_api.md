# Library

Get the list of all cards currently available in the game.

**URL** : `https://api.litemint.com:9088/library/`

**Method** : `GET`

**Auth required** : No

**Permissions required** : None

## Success Response

**Code** : `200 OK`

**Content example**

```json
{
  "card_base_00001": {
    "id": "card_base_00001",
    "type": 1,
    "name": "Overload I",
    "icon": 0,
    "points": [ 2, 0, 0, 0 ],
    "boost": 0,
    "dex": false,
    "base": true
  },
  "card_base_00002": {
    "id": "card_base_00002",
    "type": 2,
    "name": "Hellfire I",
    "icon": 1,
    "points": [ 0, 2, 0, 0 ],
    "boost": 0,
    "dex": false,
    "base": true
  }
}
```

## Notes

* The result is returned as an object (i.e. not an array).
  
  
# Get Deck

Generate and shuffle a playable deck based on a given list of cards.

**URL** : `https://api.litemint.com:9088/getdeck/`

**Method** : `PUT`

**Auth required** : No

**Permissions required** : None

**Data constraints**

```json
{
    "deck": "[Array of card ids]"
}
```

**Data examples**

```json
{
    "deck": ["card_dex_00010", "card_dex_00052"]
}
```

## Success Responses

**Condition** : Data provided is valid.

**Code** : `200 OK`

**Content example**

```json
[
"card_dex_00002", 
"card_base_00011",
"card_base_00004",
"card_base_00003",
"card_base_00002"
]
```

## Notes

* The returned deck is generated according to the current shuffle algorithm in the game.
