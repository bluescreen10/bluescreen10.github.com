---
layout: post
title: No Retry, No More Waiting
---

Usually when dealing with unreliable, slow services or both the most common answer is to either to increase the number of times you retry or increment timeout values. Both solutions would make your application slower and might have the following impact in your business:
  * Reduce customer satisfaction
  * Reduce the number of TPS your application can handle
  * Increment your exposure to a DoS attack
  * Increment operation costs

Although in some occasions may be the right answer we need to conduct a careful analysis before proceeding and most important we need data to make an inform decision. Now let's take a closer look to each problem individually.

**Unreliable Service**

Let's your application needs to talk to a 3rd party service that's it's known for being unreliable. In this case unreliable means that the error rate it's above your expectations, just to put numbers let's say error rate is 10%. First question should pop in your head should be what's the level of service I need to have? Assuming 100% is not possible then 99.9%, 98% or perhaps 95% there is no single answer it will all depend on your business. For the sake of the exercise let's say that 98% it's fine four our business.

Now digging down the maths let's start with the formula for success rate

Success_Rate = 1 - Fail_Rate  ** Number_of_Retries

There is one catch. This formula assumes that all errors are independent from each other which it is not true. Remember computers are deterministic and errors are not random or independent. The formula presented before becomes the best case scenario. Now isolating the number of retries becomes

Number_of_Retries = log ( 1 - Success_Rate ) / log ( Fail_Rate )

Success rate is the expected success rate. To put some numbers from our previous example if we want a success rate of 98% we need 2 retries, on the other hand if we want 99.9% we need 3 retries. Again this is the best case scenario and don't work for non-independent errors like outages or reproducible errors but gives you an better idea.

**Slow Service**

Now you rely on a service that is slow and X% of the requests to that system fail because timeout problems. Also the service itself may not be slow but their responses may not be consistent. For instance a system with an average response of one second but having also a large number of responses that goes above 3 seconds. To attack problems like this you need to collect more information like what is the success rate your application needs or what is the distribution of the response times. Again it will vary depending on the case.

Let's say you interact with a system with an average response time of 1sec. with a normal distribution and with a timeout of 1.5 secs you have a success rate of 80%.

![Response time distribution curve for 80% success rate][i1]

Now, let's say that you need a 95% of success rate. How much do I need to increase the timeout. In this case just increasing 0.25secs it's enough to reach the level required

![Response time distribution curve for 95% success rate][i2]
 
Would you have guessed it? The increase is barely noticeable and made a huge difference. Also depending on the form of the distribution curve you might need to increase timeout a lot to make the service level any better and changing and for example from 1sec to 3sec could leave you in almost same position.

**Conclusions**

I'm sometimes surprised as how many engineers are quick to say "increment the retry count to X" or "change the timeout to Y secs" without collecting any metric or fact. To me this is like throwing punches on the dark and far from a *real* engineering practice. Also sometimes the costs of those decisions are far more than the benefits we get in exchange. For example you increment your timeout setting and now less people use your application because it became "painfully slow". Bottom line collect data, dig down in numbers, ask questions and *always* make decisions based on facts and numbers not on guesses.

[i1]: /images/posts/normal_80_percent.png "Response time distribution curve for 80% success rate"
[i2]: /images/posts/normal_95_percent.png "Response time distribution curve for 95% success rate"
 


