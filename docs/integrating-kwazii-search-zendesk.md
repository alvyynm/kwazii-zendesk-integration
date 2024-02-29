# **Integrating Kwazii Search into Zendesk**

For every physical or software-based product, customer support plays a crucial role in acquiring new customers and retaining existing ones. But even with a budget to pay dedicated customer support representatives to stay on call 24/7, it takes time to streamline such operations.

Kwazii, a one-stop customer service data platform, addresses this problem by utilizing the power of AI, enabling you to streamline your customer support operations and gain valuable, actionable insights from your customer support data.

Through Kwazii's Generative Search API, your customers will quickly discover information thanks to its context-aware search feature that offers tailored responses. On the other hand, Kwazii Agent CoPilot API helps ease the burden on your support team by automating responses in real-time. Kwazii also offers an Interactive Analytics API that provides actionable insights from your customer data for strategic decision-making and product enhancement.

Effective search functionality, in particular, is advantageous in various ways. It saves time and enables self-service capabilities, reducing the workload on customer representatives and improving efficiency and productivity.

Kwazii Search, in particular, uses the power of generative AI to streamline information discovery by turning your customer service data into clear, actionable insights that streamline your customer support operations.

Now that you know the benefits of effective search, you'll be glad to know that you can achieve this in Kwazii Search by using its Generative Search API, which can be integrated into Zendesk. At the end of this guide, you'll have the necessary knowledge to achieve the following:

1. Create a search application in Kwazii
2. Get Kwazii's Generative Search API keys
3. Use Kwazii's Generative Search API
4. Obtain and embed Kwazii Search iframe in any user application
5. Integrate Kwazii Search into Zendesk

Let's dive right in.

## Creating a Search Application in Kwazii

You need to create a search application to get started and set a foundation for the rest of the guide. This will also allow you to use Kwazii's Generative Search API.

Follow these steps to create your first search application in Kwazii:

1. Create an account on [Kwazii](https://platform.kwazii.app/). A link will be sent to your email. Use the link in the email to log in to your account. After logging in, you'll be taken to your Home tab, which provides a step-by-step guide on how to get started.
2. First, select **Zendesk** as your data source.
   ![01-kwazii-app](/images/01-kwazii-search-app.png)
3. Then, on the next page, select **Connect to Zendesk**.
   ![02-kwazii-app](/images/02-kwazii-search-app.png)
4. On the next page, enter your preferred application name (you can choose any) and tenant (your organization's subdomain on Zendesk), then select an authentication method for signing into Zendesk and enter the required details. After that, under **Cron schedule**, select how often Kwazii should scrap your Zendesk data (either once, daily, weekly, or monthly).
   ![03-kwazii-app](/images/03-kwazii-search-app.png)
5. After that, click **Create**. A pop-up confirmation will be displayed if the login is successful and the data source is added. Kwazii will redirect you to the **Data Sources** tab, where you can see if the scraping was successful.
   ![04-kwazii-app](/images/04-kwazii-search-app.png)
6. Next, click **Applications** in the left sidebar and select **Zendesk: Knowledge Base Search**.
   ![06-kwazii-app](/images/06-kwazii-search-app.png)
7. Enter your preferred application name and **choose your Zendesk data source**. Remember, only public dataset data sources will be shown.
8. After that, enter your greetings questions (questions that will be displayed at first) and a custom placeholder text, and fill in the rest of the form if needed. Once done, click **Create** at the bottom to create your search application.
   ![07-kwazii-app](/images/07-kwazii-search.png)

A confirmation pop-up will be displayed, showcasing whether the process was successful or not. If the process is successful, Kwazii will provide an iframe you can embed in any page and a script for extending your Zendesk embeddable FAQ application.
![08-kwazii-app](/images/08-kwazii-search.png)

## Obtaining API Keys for Generative Search APIs

With an account on Kwazii, you can obtain Generative Search API by following these steps:

1. Log in to your [Kwazii account](https://platform.kwazii.app/).
2. Select **API Keys** or the **Key** icon in the left sidebar.
   ![01-kwazii-api](/images/01-kwazii-api.png)
3. Once you're in the API Keys section, select the **Add** button.
4. In the pop-up, enter the name of the key for easy identification.
   ![02-kwazii-api](/images/02-kwazii-api.png)
5. Click **OK**. After that, Kwazii will instantly generate an API key with your chosen name.
   ![03-kwazii-api](/images/03-kwazii-api.png)

To copy the API key, click the **Copy** icon. Like any other API key, please handle it with care. The first rule is to never hardcode API keys in your code. In JavaScript, the standard solution to hide your API keys is to store them as key-value pairs in .env files:

```json
// .env file
KWAZII_API_KEY='YOUR-API-KEY'
```

Then load them as environment variables via process.env like so:

```javascript
// index.js
const kwaziiApiKey = process.env.KWAZII_API_KEY;
```

Finally, create a **.gitignore** file (if you don't already have it) and add **.env** to exclude your environment variables from source control.

```javascript
// .gitignore file
.env
```

By doing so, you avoid hardcoding your API keys in your code and ultimately avoid pushing them to source control. Environment variables are a good starting point.

If you need a foolproof solution, encrypt your environment variables or use **HSM (Hardware Security Modules)** or **TPM (Trusted Platform Modules)** for more robust key storage.

## Using Kwazii's Generative Search APIs

Kwazii provides two Generative Search API endpoints: one for searching content (Content Search API) and another for data retrieval (Conversation Data Retrieval API). These Generative Search APIs provide an easy way to integrate Kwazii Search into your product.

One reason why you should use the Content Search API is to make it easy for your users to search for information. And as a company, the Conversation Data Retrieval API allows you to see what your users are searching for, which can help you understand the issues they are currently facing.

Now, let's discuss how to make API requests to Kwazii's Generative Search APIs in JavaScript.

### Content Search API

This API provides a way to search programmatically for content within applications you create in Kwazii. The API is unique because it is context-aware, making returned responses more helpful. Here's how to fetch content in your existing application in Kwazii via the Content Search API in JavaScript:

```javascript
// 1. Install and import the uuid library for generating unique IDs
import { v4 as uuidv4 } from 'uuid';

// 2. Read the API key from your .env file
const kwaziiApiKey = process.env.KWAZII_API_KEY

// 3. Make an API request using the fetch library
fetch('https://api.kwazii.app/v1/content/search/<applicationId>', {
  method: 'POST',
  body: JSON.stringify({
    conversationId: uuidv4(),
    question: "Can I play PubPod on any platform?",
  }),
  headers: {
    'Content-Type': 'application/json,
    'charset':'UTF-8',
    'X-KWAZII-API-KEY': '<kwaziiApiKey>'
  },
  redirect: 'follow'
})
.then(response => response.text())
.then(result => console.log(result))
.catch(error => console.log('error', error));
```

In the code snippet above, we make a POST request to the Content Search API endpoint. You must replace <applicationId> with your application ID in the API endpoint. You can find the application ID by going to the **Applications** tab in Kwazii and selecting your chosen app. The application ID can be found in the iframe's src attribute denoted by **applicationId='randomID'** or in the provided script indicated by `_kwazii['applicationId'] = 'randomID'`.

For every API request, you must include a unique `conversationId` and a `question` in the body. There are various ways to generate unique IDs. In the example above, we used the uuid library from npm to create random IDs.

The `question` parameter of the payload specifies the question you're seeking an answer for. Additionally, Kwazii's API requires authentication via an API key that needs to be included via the `X-KWAZII-API-KEY` header. Otherwise, an "Auth token or API key (X-KWAZII-API-KEY header) is required"error will be thrown.

Here's a sample response from the API:

```json
We regret to inform you that the PupPod Game is designed to work with specific PupPod hardware and is not a platform-agnostic game. It requires the PupPod Rocker with Feeder unit to play.

Learn more: [Does the game remember the last play session?](https://puppod.reamaze.com/kb/intro-to-puppod/does-the-game-remember-the-last-play-session) [Can I use multiple PupPod Rocker with Feeder units at the same time?](https://puppod.reamaze.com/kb/puppod-game/can-i-use-multiple-puppod-rocker-with-feeder-units-at-the-same-time)

Related Questions:
- Can I play PubPod offline?
- Are there any additional costs associated with playing PubPod on different platforms?
```

After getting the response in the `result` variable, you can then display it.

### Conversation Data Retrieval API

This API lets you retrieve content about a specific conversation in a Kwazii application by specifying the conversation ID. For instance, if you've made a search via the Content Search API, you can later retrieve conversation data about a specific conversation.

Here's how you can fetch data about past conversations in your application in Kwazii via the Conversation Data Retrieval API in JavaScript:

```javascript
// 1. Read the API key from your .env file
const kwaziiApiKey = process.env.KWAZII_API_KEY;

// 2. Make a GET API request using the fetch library
fetch(
  "https://api.kwazii.app/v1/content/search/<applicationId>/conversation/<conversationId>",
  {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
      charset: "UTF-8",
      "X-KWAZII-API-KEY": "<kwaziiApiKey>",
    },
    redirect: "follow",
  }
)
  .then((response) => response.json())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

In the code above, we make a GET request to the API endpoint. You must specify the `<applicationId>` and `<conversationId>` with actual values. Similar to the POST request in searching content via the Content Search API, you need to include your API key in the `X-KWAZII-API-KEY` header.

Here's a sample response:

```json
{
  "data": [
    {
      "answer": "-",
      "answerType": "answer",
      "question": "Can I play PubPod on my Xbox?",
      "created": 1708509095683,
      "updated": 1708509602605,
      "intentCategory": "compatibility",
      "intentFeature": "xbox compatibility",
      "intentIssue": "device compatibility"
    }
  ]
}
```

You can read the individual key-value pairs in the response data. For instance, to get the question from the response, use the format `result.data.map(item => item.question)`.

You can read Kwazii's [Generative Search APIs documentation](https://api-docs.kwazii.app/#d2651469-4364-413a-a3c0-4786f6c321ce) for more information. The documentation is also regularly updated in case of any changes.

## Embedding Kwazii Search Iframe

One quick way to embed Kwazii Search in your application is to use the provided iframe. You can embed the iframe at any position on your website. Here's how you can do it:

1. First, navigate to the **Applications** tab in Kwazii and select the search application that you created.
   ![01-embed-kwazii](/images/01-embed-kwazii.png)
2. On the next page, copy the provided iframe.
   ![02-kwazii-search-app-settings](/images/02-kwazii-search-app-settings.png)
3. Open your website or web application's source code.
4. Paste the iframe at any position on the page that you'd like your users to search for information easily.

By default, the iframe doesn't have a specific dimension. You can customize the iframe size by entering values in the height and width attributes. For instance, if you'd like your iframe to be 700px in height and 550px wide, enter 700 and 550 as the values of the height and width attributes, respectively. However, we recommend using internal or external styles to apply the height and width properties so you can easily make the iframe responsive.

Besides the iframe's size, Kwazii allows you to style the user interface to match your brand's identity. You can do this by adding custom styles directly in the style attribute of the iframe or internally or in an external CSS file. Because we're working with an iframe, you can only customize it; you don't have complete control of the internal elements.

Regardless, you can style the iframe to match your brand colors by customizing the background and border colors.

Here's an example of how you can customize the iframe:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0"
    />
    <title>iFrame Sample Kwazii - demo</title>
    <style>
      body {
        background-color: #f4d7d0;
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
      }
      iframe {
        border: 2px solid #ec5c38;
        background-color: #f4d7d0;
        border-radius: 20px;
        padding: 20px;
        height: 500px;
        width: 300px;
        align-self: center;
      }

      @media screen and (min-width: 600px) {
        iframe {
          padding: 10px 15px;
          height: 700px;
          width: 550px;
        }
      }
    </style>
  </head>
  <body>
    <h1>Kwazii Search Iframe Demo</h1>

    <iframe
      title="kwazii search"
      id="kwazii-search-iframe"
      src="https://platform.kwazii.app/ui_applications/kb_search?applicationId=applicationId&amp;apiKey=application_kwaziiApiKey"
      style="/* ... */"
      frameborder="0"
      height="..."
      width="..."
    ></iframe>
  </body>
</html>
```

<!DOCTYPE html>
<html>
  <head>
    <meta
      name= "viewport"
      content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0"
    />
    <title>iFrame Sample Kwazii - demo</title>
    <style>
      body {
        background-color: #f4d7d0;
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
      }
      iframe {
        border: 2px solid #ec5c38;
        background-color: #f4d7d0;
        border-radius: 20px;
        padding: 20px;
        height: 500px;
        width: 300px;
        align-self: center;
      }
```

We've also customized the iframe's border-radius and added some padding to ensure equal space between the border and the internal elements.

**NB**: Remember to replace `kwaziiApiKey` and `applicationId` with your own unique API key and application ID provided by Kwazii.

Here's the final result:

![02-kwazii-search-iframe](/images/02-kwazii-search-iframe.png)

## Integrating Kwazii Search into Zendesk

You can use Kwazii's context-aware search functionality to make your Zendesk Guide articles easily searchable. This can be achieved by integrating Kwazii Search into Zendesk, replacing Zendesk's built-in search feature. Follow these steps to get started:

1. Log in to your Zendesk account and navigate to the help center.
   ![01-kwazii-zendesk-integration](/images/01-kwazii-zendesk-integration.png)
2. Click the **Customize design** button (eye icon) in Zendesk's sidebar. This will take you to the theme selection page.
   ![02-kwazii-zendesk-integration](/images/02-kwazii-zendesk-integration.png)
3. Select **Customize** on the currently set theme, then on the next page, choose **Edit code** in the bottom right.
   ![03-kwazii-zendesk-integration](/images/03-kwazii-zendesk-integration.png)
4. Select `document_head.hbs`, then add the script provided by Kwazii on the application page. Here's an example script tag:

```javascript
<script src="https://cdn.kwazii.app/assets/reamaze.js">
</script>
<script type="text/javascript">
var kwazii = kwazii || { 'datasource': '' };
kwazii['apiKey'] = 'application<kwaziiApiKey>';
_kwazii['applicationId'] = <applicationId>;
</script>
```

One of the main challenges of integrating Kwazii Search into Zendesk is your account's role. You need to make sure that you have administrative privileges to do so. Otherwise, you won't be able to customize your Zendesk account's application code.

## Conclusion

In this guide, we covered how to use Kwazii Search in Zendesk, starting with the basics of creating a search application in Kwazii and obtaining your API keys. After that, we discussed how to use Kwazii's Generative Search APIs with different examples, followed by a step-by-step guide on embedding Kwazii Search iframe in any application.

Finally, we wrapped up the guide by addressing the elephant in the room: how to integrate Kwazii Search into your Zendesk account.

With Kwazii integrated into Zendesk, go ahead and try out different features available in Kwazii. If you have any feedback or questions, please drop them in the comment section below. We look forward to hearing about your experiences with using Kwazii Search in Zendesk.
