# Connect Thundra

Thundra allows users to trace their Lambda functions by plugging instrumentation libraries into their functions. Some users also want to have insights about their serverless application with minimum effort. If you're one of these users, you can plug Thundra into your AWS account and start monitoring all of your functions in your account within a couple of minutes.

After you create your Thundra account, you will be able to access the Thundra application. At this point, you don’t need to get your hands dirty with the integration. Before that, you can have a glimpse of the application with sample data.&#x20;

#### Take the guided tour

In order to get more of the sample data, you can take a guided tour about integrating your application into Thundra APM.

### Connect Your Application

![](https://lh3.googleusercontent.com/8jAGUjpj4wJxeQb6ugoMIpVhOeYoy6n5mnNFWB91At9DAikrPyBMOrQ1O0qgXoGFTNki04F28tTBxptkdNtjw2Sn1rik2kfMq7AGErhGPM1NKdXYQEaDV0EwifJEzIP\_3NYKNoCP)

In order to start connecting your application, you can click on the “here” text and see the instructions, or, you can click on the rocket sign from the sidebar. &#x20;

#### 1) Select Data Source

At this step, a page will be displayed that asks how you would like to connect your application. You can either Connect Your AWS account or Instrument Your Application Manually options and we will go through with some examples.&#x20;

**a ) Connect Your AWS Account**

**Step 1:** You can connect your AWS account and add our CloudFormation stack into your AWS easily. In order to do that click “Connect Thundra'', as shown below.&#x20;

![](https://lh6.googleusercontent.com/G5mFwbQsT3J5CUEk0F1fB3PhO4YT9yVZLEjojtdH8mxFow8pmyZ2\_g7F1FGno6r\_\_9IWbRK\_utJPX6L-8VMK44sKoxcA0wMzJR0UcZy0BeAlLdT0ceHNX\_n21rTMWqqF7RNElxBZ)

**Step 2:** You will be led to a page informing you about what needs to be done when you click the “Connect Thundra” button below. Make sure that your AWS is ready to connect.

![](https://lh5.googleusercontent.com/lvq2dIJ-ltN8OfDoBbUTms6UxWIBmYfiv82jePwrrWKVUan70DH5tik0MJGOZ3EuVzhjzt3HqGNfmADoVee5PrjwY69UV\_7VnX-gxol-TS6l9Pv568mnstURkqoO-Tpw9sa50UCO)

Thundra will help you create a CloudFormation stack when you click on the “Connect Thundra'' button. The CloudFormation creation wizard will be visible displaying information about the Thundra stack. You need to check the box called I Acknowledge that AWS CloudFormation might create IAM resources to allow access. When creation is finished, you are able to display a list of your functions.

If you’re not a technical team member with access to your AWS account, you can invite a developer by entering their email address. We're sure that you will have someone on your team who can help you to connect with Thundra.

At this point, it's worth mentioning that you can skip the "Connect Thundra" step, and manually instrument your functions.

**Step 3:**

![](https://lh3.googleusercontent.com/4t7TWwOH5uVLaTut6f0-Gdu0XhvbKg7nXzkZoIQriamcIqmSXk3QspVtNZn1HyD7U1RDu1vRH\_t3KAnr6AtzufUhI8rFXwgRt95daggQsHMLrveKenxbLMsGgWPx9hx15XogTJth)

You can select functions that you want to monitor on Thundra from the list. You may have a lot of functions, so you have the capability to search through them by runtime or name using the search bar. If you forget to select some functions, don't worry. You can always alter subscriptions of your Lambda functions from the AWS tab on the settings page.

By clicking “Autosubscribe to new function” you can let Thundra monitor your new functions as you define them. If you already have an account on Thundra, you can also connect with Thundra. First, navigate to the settings page under "User" and go to the AWS tab. On this page, you can also add new AWS accounts and you will go through the same subscription process.

![](https://lh5.googleusercontent.com/PjCFwUIRmINuuXFkadQuTI2Iu2WpYrTfSTrJA1m3y0i40eAqEuWLZIrGhRqfq7dQMUZSNUGw3rvPB\_ncOFUTmHU2DV6tsLuqrVZ\_4IUuwaacf1m-Jhlypc16zdE5GHfWynnJRSAy)

**b) Manually Instrument Your Application**

**Step 1:** If you don’t want to give access to your AWS account, you can always manually instrument your application to monitor with Thundra. Thundra supports different types of languages, platforms, web frameworks, and applications. You can select your platform from the list and follow the basic instructions to help you to start Thundra effortlessly.

![Serverless Options](https://lh4.googleusercontent.com/9UQ04SA1Anv4S7EWS0DHPInOu-ApntxKuMHl\_hSiznnyRzZnd9XZB4ld1\_OElSnvQz6QZI2e\_-0PvnCEtZFRxB5ph4zjXhkUQILtwRsnCP-gb5TmhbDvOXI\_tGnBK9qjyoDKssAo)

![Web Framework Options](https://lh5.googleusercontent.com/2AH1QQtQeI2YJ-bZu3\_NQWBFNCKdZ4qU6uaSpPq95gDaofmUeZ0K3uDOSO-aSvrZm7ErCgHb6xvdb4\_l6pYlnipVD9wkTT8PblFIvcgaRWzpomVdnvtRwji6\_e\_iptcOcMZglXtd)

**Step 2 -** After choosing your application, follow the instructions and complete your instrumentation! According to your preferences, you will be led to a page similar as follows:

![](https://lh6.googleusercontent.com/nK38RqKc3ZlGsy0KEuN1SirLFew-IfH0FYVvhRD5qp06NLNepyOnoxadvTCfnZC2hl-VXXQkCRgHdseqRGz1VHM8NtlML7sVk3tgc1LJ7rC6b0Rq-T0vp73ppkVQDtRKqIGdRbd5)

\
