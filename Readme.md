

#### To extract value for *deck_id* from JSON
```
{
   "shuffled": true,
   "remaining": 52,
   "success": true,
   "deck_id": "qdsfmvpz4hau"
}
```
`${<TestStepName>#Response#$deck_id>}`

#### To extract value for *code* from JSON
```
{
   "remaining": 51,
   "cards": [   {
      "image": "https://deckofcardsapi.com/static/img/JS.png",
      "images":       {
         "svg": "https://deckofcardsapi.com/static/img/JS.svg",
         "png": "https://deckofcardsapi.com/static/img/JS.png"
      },
      "value": "JACK",
      "suit": "SPADES",
      "code": "JS"
   }],
   "success": true,
   "deck_id": "i2uawfwsc6kk"
}
```

`${<TestStepName>#Response#$cards[0].code}`

### Useful links

- https://www.soapui.org/resources/tutorials/rest-sample-project.html?utm_source=soapui&utm_medium=starterpage
- https://api.carbonintensity.org.uk/
- https://deckofcardsapi.com/


