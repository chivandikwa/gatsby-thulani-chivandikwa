---
layout: post
title: Bot Time
image: img/computer.jpg
author: Thulani S. Chivandikwa
date: 2019-03-10T10:00:00.000Z
tags: [Microsoft Bot Framework, LUIS, Chat Bot, C#]
draft: false
---

### Microsoft Bot Framework and LUIS

Back in 2017 a colleague at the time introduced me to the [Microsoft Bot Framework](https://dev.botframework.com/) and the [LUIS (Language Understanding) AI](https://www.luis.ai/home). Microsoft Bot Framework is a comprehensive framework for building enterprise-grade conversational AI experiences. What is attractive about this framework to me is how easy it is to interface with multiple channels like Slack, Telegram, Teams, Skype, you name it.

LUIS on the other hand is a machine learning-based service to build natural language into apps, bots, and IoT devices. LUIS exposes and easy way to implement natural language processing by creating intents and entities that you can then link together with utterance/phrases to train your bot with.

Have the Bot Framework handle your conversational experience and use LUIS to do natural language processing and you can easily have a Bot up in no time. While natural language processing is great, I must point out that it is not always necessary and typed conversations can be a problem for a user and lack guidance. The Bot Framework makes it easy to handle cases where you want to guide the user with cards and buttons. This will be the first of a series of posts where I share my experiences as I get back into playing with the Bot Framework and LUIS.

### Let's get this show on the road

So how shall we do this? Well...I shall skip the hello world type of examples and jump right into something more fun. If you want to take it from the basics have a look at the documentation and some samples [here](https://github.com/microsoft/BotBuilder-Samples). I shall be using C# but the Bot Framework has support for Node.js as well.

For our case let's create a bot that will be able to handle user typed requests for the weather. Yep, let's jump right into natural language processing! But first let's looks at how we shall layout our application.
