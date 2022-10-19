# GCPHackathon
## Content
- [Overview](#overview)
- [Dialogflow setup](#dialogflow-setup)
- [Architecture](#architecture)
- [Appendix: Destination images and similar destinations Suggestion Chips](#appendix-destination-images-and-similar-destinations-suggestion-chips)


## Overview

The aim of this project is to get User's **queries about how much is a flight to a destination** using Google Assistant, to **get the lowest fare** from [Amadeus](https://amadeus.com/en/industries/airlines), and **reply back** to the User. 

The User can interact with the assistant via **voice**, **keyboard** or **touching** the device screen.

This way of getting travel inspiration has many benefits in terms of **accessibility** compared to traditional booking tools offered by the airlines websites. In addition, this new channel is much faster.

![Screenshot](/assets/images/comparison.png)

Thanks to Dialogflow, this application could work as well in **other languages**. We would just need to provide more training sentences, and then translate some parameters using Google Translation API. Dialogflow will get the `period` correctly with same format `YYYY-MM-DDTHH:MM:SS+00:00` as in English.

Steps:
- User can select **Airline** *I want to go to Lahore*
- user can select **Origin** *I want to fly from Lahore to Islamabad*
- Add more inspiration capabilities: i.e. *'I want to fly to a sunny place in December'*
- Allow more inputs from the User: *'Flights to Lahore for 2 adults and 1 infant in Business'*
- Use context not only carrying dates and inputing a new destination, but viceversa: i.e User: *'I want to fly to London next weekend'*, (agent response), *'And for the last weekend of November?'*

Some examples:
## Dialogflow setup
(Pending) [here](https://cloud.google.com/dialogflow/docs/)

Make sure you enable Webhook calls for the Intent.

In this example we create two follow-up intentes directly from Dialogflow console (not fulfullment) that simulate the User asking *'Hey, email me this info'* or *'Please book me in this flight'*.

Find some screenshots in the [appendix](#appendix:-dialogflow-screenshots)

We use context to carry information between consecutive interactions:

![Screenshot](/assets/images/contextdiagram.png)


## Architecture
(Pending)

![Screenshot](/assets/images/NEWflow.png)


#### Welcome and Fallback functions.

Welcome Intent: triggered when the user starts the conversation.

Fallback Intent: triggered when the Agent can't match the query with any Intent

In both cases, we want the user to get some [Suggestion Chips](https://developers.google.com/actions/assistant/responses) to give some inspiration.

(Pending)

The Amadeus API has a standard POST/GET interaction, you can read more about it [here](https://developers.amadeus.com/self-service/category/air/api-doc/flight-low-fare-search/api-reference). You will need to create a test account to get your `client_id` and `client_secret`.

First, using a `POST` request we get the `access_token`. Then, with a `GET` request we ask for the json file containing flights and fares. 

Here you can see an example of [Amadeus API response](AmadeusResponseExample.json)
 

## Appendix: Destination images and similar destinations Suggestion Chips

The main database we will use is stored in Firestore. We will load a [reference table](/aIrport_codes_200.csv) for 200 destinations.

Each destination in the database has an **image url** and a list of **similar destinations**.

You could build this database with your own images and calculate the similarity based on users behaviour on your website, geographic proximity, themes (beach, ski, nature...) , etc. In this example we will use an already built database from [Nomadlist.com](https://nomadlist.com), a nice website with lots of info about places to visit. 

