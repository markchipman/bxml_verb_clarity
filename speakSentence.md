
## XML: `<SpeakSentence>`
The SpeakSentence verb is used to convert any text into speak for the caller.


### Attributes
| ATTRIBUTE | DESCRIPTION                                                     |
|:----------|:----------------------------------------------------------------|
| gender    | (required) Select the gender of the speaker (default=”female”). |
| locale    | Select the accent of the speaker (default=”en_US”).             |
| voice     | (required) Select the voice of the speaker, limited by gender.  |

| **gender** | **locale** | **voice** |
|:-----------|:-----------|:----------|
| female     | en_US      | susan     |
|            |            | julie     |
|            |            | kate      |
|            | en_UK      | bridget   |
|            | de         | katrin    |
|            | de_de      | katrin    |
|            | es         | esperanza |
|            |            | violeta   |
|            | es_mx      | esperanza |
|            |            | violeta   |
|            | fr         | jolie     |
|            | fr_fr      | jolie     |
|            | it         | paola     |
|            | it_it      | paola     |
| male       | en_US      | paul      |
|            |            | dave      |
|            | en_UK      | simon     |
|            | de         | stefan    |
|            | de_de      | stefan    |
|            | es         | jorge     |
|            | es_mx      | jorge     |
|            | fr         | bernard   |
|            | fr_fr      | bernard   |
|            | it         | luca      |
|            | it_it      | luca      |


### Events Recevied

| Event | Can reply with more BXML |
|:------|:-------------------------|
| None  | No                       |


#### Example:  SpeakSentence Verb
This shows how to use Bandwidth XML to use text to speech to speak a sentence into a phone call.

```XML
<?xml version="1.0" encoding="UTF-8"?>

<Response>

<SpeakSentence voice="paola" locale="it" gender="female">
Questo è un test
</SpeakSentence>

</Response>
```


