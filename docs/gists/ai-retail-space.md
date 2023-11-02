# Overview of Retail Chatbot Interactivity
Trevor Wieland 2023-10-09

## Purpose Statement
With all the chaotic newspieces about AI interactivity coming any day now to the retail space, it's important to take a step back and get perspective on what is really important.
There has been a period of massive development in accessible AI training and deployments, which in some ways causes confusion over what is actually helpful to the customer experience.
The purpose of this gist is to walk through a few concepts that are being thrown around right now, and break things into varying levels of complexity in terms of the deployment effort.
In doing this, hopefully less AI-tech-savvy but still very interested parties can get a better understanding of what the actual options are, and what improvement to the user experience (if any) these solutions provide.
While this gist will mostly take the perspective of the typical retail user, and thus guide talking points further in that field, the same core concepts should apply fairly generally.

## The User Experience 
At the end of the day, in the retail industry, there are mostly two real metrics that matter in terms of user experience:
- Conversion Rate (What % of users actually make a purchase)
- Return Rate (What % of purchasing users later return their purchase)

There are of course many other metrics that will matter in varying amounts to different sub-industrys or even different stores, but generally these two are the most "experience" driven metrics.
Whether a user has a positive experience shopping at your store doesn't directly impact your logistics costs, but it certainly impacts the opinions that the user maintains about their purchase.

Using these two metrics as our guide, we'll look at a few emerging technologies using AI, as well as break down some of the levels of complexity that we could choose to use, to assess whether each step is worth the added costs.

## Case Studies
We'll take a look at this problem by moving through various case studies, various upcoming and current technologies that are being deployed by others in the retail industry.

- [Chat Bots](#chat-bots)
- [Voice Synthesis](#voice-synthesis)
- [Mascot Interaction](#mascot-interaction)

### Chat Bots
This is perhaps the most pressing and impressive emerging tech for the retail industry, but possibly also the most misused.
It feels like every company, big or small, is working on adding intelligent chatbots to their ecommerce (or even brick and mortar!) platforms.
In this case study, we'll look at a few of the options that are available to the average retail development team, in three levels of complexity, and assess each one for its costs and its benefits.

These levels will be:
- Directly using a Pre-Trained Chatbot (ChatGPT/Bard/Claude/etc)
- Pipelining a Pre-Trained Chatbot
- Pipelining a Custom Chatbot

While I will discuss Pipelining in more detail at those levels, in short, it is building a larger codebase around the model, that uses potentially multiple models with the purpose of improving the UX of a chatbot.

#### Direct Chatbot Usage
If you've ever wondered how these AI chatbot compaines are making their money (other than billions from other companies investing), this is how.
Nearly all the big players in the Pre-Trained chatbot space offer low-cost API access to their models.
This means that anyone with at least some budget and at least some programming ability (or can google some basics) has access to these pretrained models to apply to whatever problem they have.
For more details about implementing this yourself, see some of the following resources:
- [OpenAI ChatGPT](https://platform.openai.com/docs/introduction)
- [Anthropic Claude](https://docs.anthropic.com/claude/reference/getting-started-with-the-api)

A lot of the discussion on these models focuses around which model performs the best, or has the most human-sounding responses, or which model makes the most random stuff up (hallucinations), however none of these are actually directly related to the user experience as we defined it above. Instead, it's up to you to determine in what way to best use these technologies to actually improve the user experience. 

So, for these direct chatbots, all they are capable of doing on their own is responding to text input with more-or-less human-sounding output text. Thinking in the retail industry, you might immediately jump to thinking about customer service as simply taking in input text (user questions/complaints) and returning output text (customer service responses), but in reality, even customer service involves more steps than just this chatting. Customer Service Representatives also typically have access to pre-defined scripts to match their responses against. Additionally, if there is a real problem being raised by the customer, ticketing systems are often used as well, with escalations being acted on and decisions being made. 

These are all things you don't want to leave to a pretrained text model to decide for you, even if it is just a question the user is asking. For example, what if a user were to ask a chatbot about warranty information for a product they purchased, and the chatbot 'hallucinates' that the product has a warranty twice as long as it actually is supposed to be? Is your company prepared to honor that supposed warranty if they later bring it up?

From a UX perspective, having a pretrained raw-chatbot provides little in the way of converting prospective users, and considering any specific answers given to user questions will likely be hallucinated by the model, will likely do nothing to improve return rate of purchased products. It is for this reason, that "having a chatbot just to have one" is a bad idea. While this method is a low cost and low turnaround time project, it is unlikely to improve anything for the user, and thus not worth the investment 

With these realizations in place, the limits of large language models on their own start to become more apparent. But luckily, there are ways of dealing with these problems, albeit with greater costs and levels of complexity.

#### Pipelining a Pre-Trained Chatbot
Large Language models in general have been shown to tackle a large variety of problems with surprising accuracy, given that they have only been trained to generate text by looking at previous text tokens. Therein lies the feature of these large models that is working best for developers in the retail industry and beyond.

Instead of sending messages to the chatbot API using this process:
- User Inputs Text
- Create Prompt
    - ex: "You are a retail customer service rep... USER: USER_TEXT. YOU: "
- Return the output text to the user

We instead use this flow instead:
- User Inputs Text
- Create a Categorization Prompt:
    - ex: "Which of the following categories [CAT1, CAT2, CAT3] is this user request: USER_TEXT"
- Run a specific Category Prompt:
    - ex: "What is the name of the product that the user is asking a question about in this text: USER_TEXT"
- Repeat until fine grain enough
- Construct a final Prompt to respond accurately
    - ex: "You are a retail customer service rep... The user has asked for details about this product PRODUCT. Our database has this information PRODUCT_DATA. Please summarize details about this product. YOU: "
- Return the final output text to the user

This flow can be customized and adapted to your specific flow, as obviously every use case will need different categorization and steps to provide the best results. The advantages of this approach are significant:
- We can reduce hallucination by only ever asking targetted questions
- We can measure the individual accuracy of each step, meaning we now know the real relevant accuracy of these models for our use case
- We can easily tweak individual parts of our flow as needed without changing the entire model

However, this method adds significant complexity to our procedures. You would no longer be able to just send user text asis to the API. Additionally, time would need to spent crafting each prompt step for each part of the flowchart. 

Lastly, perhaps the most unavoidable downside of this method, is that with each step you add to the process, you have to send another api request to get the answer for that respective step. If your flowchart has an average length of 10 steps, you could potentially be increasing your costs by a factor of 10x (or more) compared to some ideal model that takes a user question and handles the answer with a single api request. With each mandatory API request, you are adding cost and latency to your chatbots reply that will quickly add up to become significantly more than the alternative.

Altogether, even with the downsides, this is probably the generally correct approach to take for most businesses today. It provides the opportunity to have a state of the art chatbot with no AI-training at all. And while costs are larger than having a direct-to-model approach, the turnaround time and development costs are significantly lower than if you needed to deploy your own trained model.

From a UX perspective, this method provides the flexibility we need to start addressing real user-problems, with the added benefit that part of our user-flow can be designed to escalate to a human rep if needed, so that we cover our edge-cases. With a bit of creativity, we can create flows that upsell clients more profitable versions of the products they're looking for, check inventory at the store closest to them, provide commentary on the items on their cart with any additional recommendations, and so much more. By choosing this method, the limits are your own creativity, and dealing with increasing costs as more sequential steps get added to the pipeline.

#### Pipelining a Custom Chatbot

In this use case, we structure everything similarly to the above section, but now we replace whichever pre-trained chatbot model with one of our own. This has the potential to provide even greater performance, which we can measure at each step of the flow individually or alltogether. The cost, of course, is needing to provide both training data for your model, and to find the right hosting solution for your model.

For training your model, we would want samples matching the same prompt-response examples that you would be using for your user-flow that is used in production. Without a chatbot already in place, this data can be hard to come by. After all, if you are the one designing the user-flow to solve a unique problem your business faces, there is not likely to be any publicly available dataset that contains training data to solve your problem. This leaves you with two options that I would recommend: 
- You could generate example sentences manually for each flow-step
    - EX: question:'Which of the following categories ["Groceries", "Pharmacy"] is the user talking about: "Is my prescription ready for pickup"? Answer with the format \<ANSWER\>.' 
    - EX: answer: '\<Pharmacy>'
    - By creating dozens of question/answer pairs for each step in your flow, you could then finetune a model on this small dataset and measure any improvement in each step's accuracy.
- You could implement a pretrained model, but log user questions and feedback to build a training set that way
    - The feedback could range from vague feedback about the process leaving the user happy with the experience, to specific feedback about specific steps going wrong
    - Real Data >>> Generated Data
    - Depending on usage amount, training data could take a long time to roll in, especially if few users take the survey

Of course, there is no reason why you can't try one or both of these and see what works for your specific use-case. The main takeaway that I would like you to understand however is that finetuning a model for better performance isn't the first step to a good user-experience however. After all, if you could only pick one, between fine tuning a model, or constructing a prompt-flow as described above, the prompt-flow will win everytime in terms of accomplishing the goals you set out for the chat bot.

The remaining problem to deal with is model deployment. After all, the deployment of models like ChatGPT is already handled for you, all you need to do is access the API with simple POST requests. When you train your own model, you're then responsible for hosting it, which adds a new kind of cost. At every step in this process, I highly recommend conducting a cost-benefit analysis to see if the added cost of a hosting solution will bring in enough added benefit to offset it.

From a UX perspective, however, this is the best that you can possibly do with an AI chatbot. Finetuning the model to improve the accuracy of each individual component of the user flow will allow to to fix any problem areas that your model is struggling to understand, not to mention that the act of processing user-feedback will highlight any gaps in your designed flow, allowing you to adjust your flow in accordance with which features your users actually use. 

### Voice Synthesis
tbd

### Mascot Interaction
On the topic of user interaction, despite the large volume of discussion about text and audio integrations for AI-assisted user experience handling, there is relatively little really discussed about the visual component. By this, I mean the inclusion of a mascot of some sort being 'part' of the conversation. Maybe it is the case that most people consider the concept to be not that interesting, but I think they're missing a key opportunity that having a mascot be part of the chatbot experience provides. This provides the opportunity to visually guide the user in their purchasing experience, as once you have a visual component in the form of a mascot, it naturally invites the potential to have the mascot point to images of products that the user asks about, or to show how to interact with parts of the page better.

In this section, I'll discuss three levels of mascot chatbot integration, being the following:
- No Mascot
- Static Mascot
- Motion Mascot

Hopefully, at each step I will be able to effectively explain what each increasing level of complexity adds, from both a cost and benefit to the UX perspective.

#### No Mascot
The easiest solution to what you should do about having a mascot associated with your chatbot is clearly not to have one at all. This saves time, money, and space on your website, and certainly won't degrade the accuracy of your chatbot experience in any way. However, this also means that without much of a visual representation, catching the user's attention that there is a chatbot even available to talk to becomes a touchier subject. No one likes the popup-style of chatbot integration to a website, with the fake notification. It feels like spam because in a way it kind of is if the user isn't wanting to talk with a chatbot.

By including some sort of mascot in a corner of the website labeled as a chatbot, you naturally catch the user's eye without needing to resort to more flashy actions. Users that want to interact with the website through chat will see it and know its there as an option, and users that don't want to interact in this way will ignore it, scrolling by without feeling harassed into using the fancy new tool that you've invested so much time and money into.

Additionally, assuming that the user has clicked into the chatbot, and is now interacting, having a mascot be part of the page will naturally encourage a design that takes a bit more space on the page. This has the drawback of potentially making mobile device interactions with your chatbot no longer able to browse the page as well. Since some use cases of the chat bot may encourage further use of the website in addition to talking with the chat bot (For example, if we wished to have a chatbot that could answer product questions, we would want the user to still be able to browse to products on their own), if we take too much space with our chatbot design, we will impede natural use of the website.

From a UX perspective, people are used to browsing websites on their own, not needing their hand to be held by a chatbot doing the product browsing for them. Because of this, when we design a chatbot, we need to know who we're designing for. If the purpose of your chatbot is to enable elderly or otherwise tech-illiterate users to browse the page, by all means you can safely ignore most of these issues. My point is primarily that the decision to include a mascot or not should be based on your target audience, and what you are planning the chatbot's use cases to be, and whether these are aiming to replace or improve traditional browsing.

#### Static Mascot
The above section talked a lot about the impacts of having a mascot at all, but what is it exactly that having one, even just a series of static images, will allow us to do?

For one, designing around including visuals means that we have a nice starting point to tailor our visuals to the specific interaction the user is wanting. If we design our chatbot to follow specific user flows (See [Here](#pipelining-a-pre-trained-chatbot) for details), we can design corresponding visuals to go along with each step in the flow. 

For example, let's say we have a situation where the user is shopping for paint from a hardware store, and we have designed a flow such that at our current step, the user is describing a color, and the chatbot is tasked with identifying which color to recommend to the user. If we are designing our flow with the idea in mind that this is a visual experience, we can craft this step to instead recommend, for instance, three colors of paint and show images of the color on screen. The next step of the flow would then be to analyze the user text response (or even to accept user clicking on an image as an answer) to either move on with the selection to ask about quantity / etc, or to repeat with new recommendations based on the users response. If we were instead constraining our chatbot flow to deal with text only, it would be a much more awkward experience trying to recommend a user a color.

Note that in this example, the mascot itself plays no part in the recommendation being any better than text. However, just by having a mascot, we are inviting the user to engage with an otherwise text-based method of communication in a visual way, and we also then think about visuals when designing the flow. This is not to say that we are ditching accessibility though, as we must still make sure that anything being communicated should be interactible through text as well.

As for the added costs of this approach, we would just be creating images as needed for each step in the flow. The more complicated the flow, the more different static images of our mascot we would want. For instance, we might want one image for the introduction of the chatbot, another with a pose for showing product display images, another for pointing to a tab on the website for guidance, yet another for waving goodbye, etc.. In any case, there is a clearly finite amount of images we would need, with most of the variance coming from just changing what we display *next* to the mascot (e.g. different colors of paint), which can be trivially handled.

From a UX perspective, this provides new ways of users interacting with the chatbot, particularly for users that might otherwise not care about the chatbot at all. And if you design the mascot visuals around an already effective chat-flow as described in the chatbot section, you can further improve the accuracy of individual components to the chatbot experience.

#### Motion Mascot
This concept is the one that I think has gained the most hype in recent times, as the idea of having an animated figure talking in real time to the user is straight out of a scifi movie. Unfortunately I think it's also by far the least practical approach to this problem. But first, lets talk about its upsides.

Having an interactive moving mascot as part of the chatbot interaction definitely catches the eye more than a static one ever can. Not to mention the fact that there is more software than ever for the automated creation of model movement, with technology in gaming, movies, and live-streaming driving a lot of accessibility in the marketplace. This would by no arguement be the most visually impressive form of interacting with a retail website, but its benefits for a chatbot more or less end there.

The primary factor of chatbots that make them nice to have in modern times is the ability to have a truly custom-tailored experience for the user. The user comes to the chatbot with one or more goals in mind, and with AI the chatbot can help to accomplish these goals, or even more that the user didn't know they wanted. Flowcharting, finetuning, crafting visuals are all ways of making that experience more tailored for being able to solve the user's problem. The issue with motion graphics compared to static is that they don't really bring a further benefit in this same vein. 

While motion graphics might *look nicer*, they don't actually *do* anything to help solve the users problem. You might see an uptick in users actually using the chatbot, but relative to all the other problems you have to tackle if you want an effective chatbot experience, it's the highest cost for lowest reward option.

It all comes down to the fact that there are two main problems with real-time model rendering:
- Financial Cost
- Computing Cost

There is both a financial cost to creating the 3D model that you want to animate (someone has to design and rig a skeleton for the model), and a cost to the training of a model to do realtime (or even approximate) lipsyncing for this model. In gaming, the vast majority of animations are pre-rendered, because there just isn't that much extra benefit to trying to animate characters in realtime, and the remaining cases that make sense often have proprietary tools, costing large amounts to license.

Unless the intention is just to have some basic animations that don't change based on user input (at which point I'd basically just consider them static gifs anyways), this also requires computational power to render the actual model on the user's machine. So while you can certainly dream up any kind of unique, auto-lip-syncing model technology you want, everything that is out there today costs money and more importantly GPU time to render in the first place. 

One option now for AI lipsyncing in multiple languages [NeuralSync](https://www.neuralsyncai.com/) costs between $3-10 on average per video, and requires a pre-recorded video as input to lip-sync. Tools built for live rigging a model itself also tend to be built with the expectation that you are doing this as a convenience in your 3D animation software, running on a high-powered machine. Similar to how photoshop has introduced image-generation AI into their tool as a plugin, most efforts in AI animation are following a similar route, and for good reason. If you can operate on the assumption that your model will be run on a high-compute machine, you can ramp up the size of the model for higher performance without much consideration.

As you can probably guess, from a UX perspective there is not much benefit gained with potentially large downsides in terms of development costs. Because of this, I recommend steering away from live-animating your virtual assistant/mascot, and focussing more on the decision between either no mascot for a text-only approach, or a static mascor for a combination approach.