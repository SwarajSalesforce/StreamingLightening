Streaming API Lightning Component

Lightning Component for the Salesforce Streaming API based on the examples here.

 Deploy to Salesforce
Documentation

This Lightning Component makes it easier to use the Streaming API and thus Platform Events in your Lightning Components. Simply state the channel, e.g. /topic/mytopic or /event/MyEvent__e along with an onMessage handler.

<aura:component implements="flexipage:availableForAllPageTypes">
    <aura:attribute name="lastMessagePayload" type="String" access="private"/>
    <c:streaming channel="/event/MyEvent__e" onMessage="{!c.handleMessage}"/>
    <div>{!v.lastMessagePayload}</div>
</aura:component>
({
	handleMessage : function(component, event, helper) {
	    component.set("v.lastMessagePayload", JSON.stringify(event.getParam("payload")));
	}
})
NOTE: This component will work with PushTopic's as well.

Demo

Instructions

Deploy via the button above (to Summer'17 org)
Create a Platform Event with an API name of MyEvent__e
Add a field with an API name of Message__c
Add the StreamingDemo component to a Lightning Page via Lightning App Builder
Issue the following code from Execute Annonymous
Observe the message in the Lightning page created above
// Create event to publish
List<MyEvent__e> events = new List<MyEvent__e>();
events.add(new MyEvent__e(Message__c = 'My Message'));

// Call method to publish events
List<Database.SaveResult> results = EventBus.publish(events);

// Inspect publishing result for each event
for (Database.SaveResult sr : results) {
    if (sr.isSuccess()) {
        System.debug('Successfully published event.');
    } else {
        for(Database.Error err : sr.getErrors()) {
            System.debug('Error returned: ' +
                        err.getStatusCode() +
                        err.getMessage());
        }
    }       
}
