# Messenger Analytics: Creating And Logging Your Own Custom Events

![kon-karampelas-HUBofEFQ6CA-unsplash](https://user-images.githubusercontent.com/48386390/97094816-f2631180-1647-11eb-8e64-4701f288a31c.jpg)

Event logging is a crucial part of understanding how people are using your messenger bot, and how to potentially improve the experience for your users. Which is why it is a good thing that the messenger platform automatically logs a number of events from your chatbot.

There are situations however where you want to know more than your messenger bot automatically logs. Say you want to know how many people scheduled a counselling session from your messenger platform. This is where you need to create your own custom events for them to get logged.

In this short tutorial we’re going learn to create our own custom events bh doing so for a book store’s messenger bot. We’re going  to be using https://www.facebook.com/analytics?ref=analytics-blog_give-your-team-easy-access to view our custom events.

Our bot is supposed to assist visitors borrow books, purchase books and get book suggestions based on their interests. We’ll keep it really simple, to help us focus on seeing how you can create and log events that are specific to your business.

To code along with me, you can get all set by getting the webhook from [here](https://github.com/Shulammite-Aso/messenger-webhook/tree/starter_code).
I had built most of the starter code, including creating a facebook page and app from following the getting started guide on the doc (https://developers.facebook.com/docs/messenger-platform/getting-started).
Consider following the same guide to build it from scratch if you’re new to messenger platform and want to see how you can get started from scratch.

If you’ll rather see the final code that we’ll have today now, you can get it here (main branch).

For the starter code, clone it and open it in your favourite text editor.

Create the facebook page and developer app that you're going to use for this tutorial.

Next step is to deploy the webhook to any server of your choice where it can accept requests over HTTPS.  
I’ll be deploying mine to heroku (https://devcenter.heroku.com/articles/git).

Remember to set your environmental variables so that the following will resolve to your own access token, page id and app id:

```
 // Add page access token
 const PAGE_ACCESS_TOKEN = process.env.PAGE_ACCESS_TOKEN;
 // Add page id
 const PAGE_ID = process.env.PAGE_ID;
 // Add your app id
 const APP_ID = process.env.APP_ID;
```

Then Integrate the webhook into your facebook developer app.

### Create the chatflow

To be able to trigger the events you want to log, you need to get your bot to send your users a message that will most likely get them to respond with 
the event you want to record. Your message can be a text or a structured message using a message template.

We'll go ahead to create this chatflow for our bot, but before then, our starter code is using 
built-in NLP(https://developers.facebook.com/docs/messenger-platform/built-in-nlp), you will need to first of all enable built-in NLP for your app:

- Go to your Facebook app's 'Messenger Settings' page.
- Finf built-in NLP somewhere down the page and toggle the "on off" switch to enable/disable Built-in NLP for your app.

Back on your text editor, copy and use the code below to replace what is on the handleMessage function:

```
 function handleMessage(sender_psid, received_message) {

  let response;

  // Check if the message contains text
  if (received_message.text) {    

    // check greeting is here and is confident
  const greeting = firstTrait(received_message.nlp, 'wit$greetings');
  if (greeting && greeting.confidence > 0.8) {
    response = {
        "attachment":{
          "type":"template",
          "payload":{
            "template_type":"button",
            "text":"Hello! welcome to Nora bookshop. I'm here to assist you get started with your next reading adventure. Please choose what you would like to do below.",
            "buttons":[
              {
                "type": "postback",
                "title": "Buy a book",
                "payload": "buy a book",
              },
              {
                "type": "postback",
                "title": "Borrow a book",
                "payload": "borrow a book",
              },
              {
                "type": "postback",
                "title": "Get book suggestions",
                "payload": "get book suggestions",
              }
            ],
          }
        }
      }

    

  } else { 
    // default logic
    response = {
      "attachment":{
        "type":"template",
        "payload":{
          "template_type":"button",
          "text":"Hello! i'm a chat bot, and i'm here to asist you buy a book from our bookstore, borrow a book or suggest books for you based on what you tell me is your interest. so now, tell me which you want to do by selecting one of the options below." ,
          "buttons":[
            {
              "type": "postback",
              "title": "Buy a book",
              "payload": "buy a book",
            },
            {
              "type": "postback",
              "title": "Borrow a book",
              "payload": "borrow a book",
            },
            {
              "type": "postback",
              "title": "Get book suggestions",
              "payload": "get book suggestions",
            }
          ],
        }
      }
    } 
  }
}  
  // Sends the response message
  callSendAPI(sender_psid, response);    
}
```

From the above code above, the users response will be sent as a postback. So let's handle the postback with a simple response.

Copy and paste the code below into the handlePostback function.

```
let response;
  
  // Get the payload for the postback
  let payload = received_postback.payload;

  // Set the response for postback payload
  if (payload) {
    response = { "text": "Okay. we're almost set!" }
    
    // Send the message to acknowledge the postback
  callSendAPI(sender_psid, response);
    }
```
### Create your custom event

I'm 


