# BXML Verb Clarity
This is the set of documentation/requirements for Slingshot. 

## Call Processing
### Outbound Call
1. Customer uses the API to create an outbound call. Specifies the action URL. 
1. Slingshot places the call. When the call is answered, Slingshot reuests BXML from action URL
1. Slingshot executes verbs in the BXML. 
1. Slingshot requests new BXML at the verb::actionUrl if present; 
1. Slingshot continues with the next verb if no actionUrl is specified in the previous verb. 

### Inbound Call
1. Slingshot gets and inbound call
1. Slingshot looks up the app for the given appId. Get the URL
1. Slingshot requests BXML for incomingCall from the app::actionUrl. 
1. Slingshot executes the recieved BXML. 

## Verbs

| Verb                                  | Description                                                                                                                                     | Comments |
|:--------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------|:--------:|
| [`<Conference>`](transfer.md)         | Create Conferences                                                                               |TBD|
| [`<Gather>`](gather.md)               | The Gather verb is used to collect digits for some period of time.                                                                              ||
| [`<Hangup>`](hangup.md)               | The Hangup verb is used to hangup current call.                                                                                                 || 
| [`<Pause>`](pause.md)                 | Pause is a verb to specify the length of seconds to wait before executing the next verb.                   |New in Slingshot|
| [`<PlayAudio>`](playAudio.md)         | The PlayAudio verb is used to play an audio file in the call.                                                                                   ||
| [`<Record>`](record.md)               | The Record verb allows call recording. At the end of the call, a call recording event containing the media with recorded audio URL is generated |Updated in Slingshot. Used to Record calls.|
| [`<RecordVM>`](recordvm.md)           | The RecordVM verb allows recording a brief voice message. At the end of the message, a call recording event containing the media with recorded audio URL is generated |New in Slingshot|
| [`<Redirect>`](redirect.md)           | The Redirect verb is used to redirect the current XML execution to another URL.                                                                 ||
| [`<Reject>`](reject.md)               | The Reject verb is used to reject incoming calls.                                                       |New in Slingshot|
| [`<SpeakSentence>`](speakSentence.md) | The SpeakSentence verb is used to convert any text into speak for the caller.                                                                   ||
| [`<Transfer>`](transfer.md)           | The Transfer verb is used to transfer the call to another number.                                                                               ||


## Events/Callbacks

| Event                                         | Description                                                                                                                                                                                    | Comments|
|:----------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--|
| [Answer Event](events/answer.md)              | Bandwidth API sends this message to the application when the call is answered.                                                                                                                 ||
| [Gather event](events/gather.md)              | Bandwidth API generates a gather event when the gather command completes in a call.                                                                                                            ||
| [Hangup Event](events/hangup.md)              | Bandwidth API sends this message to the application when the call ends.                                                                                                                        |Not available in Slingshot|
| [Incoming Call Event](events/incomingCall.md) | Bandwidth API sends this message to the application when an incoming call arrives. For incoming call the callback set is the one related to the Application associated with the called number. ||
| [Recording event](events/recording.md)        | Bandwidth API sends this event to the application when an the recording media file is saved or an error occurs while saving it.                                                                ||
| [Redirect event](events/redirect.md)          | Bandwidth API sends this event to the application when a `<Redirect>` is requested                                                                                                             ||
| [Incoming SMS event](events/incomingSMS.md)   | Bandwidth API sends this event to the application when an SMS is sent to the Bandwidth Number assigned to an application                                                                       |Not supported|
| [Transfer Complete Event](events/transfer.md) | Bandwidth API sends this event to the application when the `<Transfer>`is complete                                                                                                             | |
| Conference Events                             | Events during conferencing |TBD|


## Use cases

How would I accomplish these common use cases with BXML

### Scenario 1: Transfer and survey
Common use case is to have a customer enter an IVR to direct them to a customer service rep.  Once the rep handles the call, they hangup and then send the original caller to an optional survey.
This requires that the incoming call stays alive after the transfered call completes.

1. Incoming call, reply with [`<Gather>`](gather.md) BXML to direct to correct department
2. Receive the [Gather](events/gather.md) callback
    1. The reply may have a BXML for another <Gather> to be able to build the IVR system. 
3. Reply with [`<Transfer>`](transfer.md) BXML based on customer response.
4. Customer and rep talk
5. Rep hangs up. Receive the [transferComplete](events/transfer.md) event
6. Reply with [`<Gather>`](gather.md) BXML to get feedback

### Scenario 2: Transfer and voicemail
Common use case is to transfer a call, if the transfer times out send the original caller to voicemail

1. Incoming call, reply [`<Transfer>`](transfer.md) BXML with `callTimeout` set
2. Receive the [transferComplete](events/transfer.md) event when transfered call doesn't answer
3. Reply with [`<SpeakSentence>`](speakSentence.md) and [`<Record>`](record.md) BXML to get voicemail
4. (Customer hangsup? or do we hangup? how do we determine when to end the call after record?)
5. Receive [Recording](events/recording.md) and [Hangup](events/hangup.md) event.
