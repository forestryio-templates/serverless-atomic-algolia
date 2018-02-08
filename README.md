# Atomic Algolia Serverless

A serverless function for updating one or more Algolia indices from JSON endpoints using a simple Webtask function.

## Installation

You must first have Node & NPM installed.

Once installed, run:

```
npm install serverless -g && npm install
```

## Configuration

Before you can use this function, you need to authorize with the provider (Webtask by Auth0). This is extremely simple, just run:

```
serverless config credentials --provider webtasks
```

You will be asked for a phone number or email. You'll immediately receive a verification code. Enter the verification code and your profile will be entirely setup and ready to use.

---

This function needs the following to be configured:

* A `secrets.yml` file with your Algolia Application ID and secret access key.
* An `indexes.js` file with a list of your indexes and their JSON endpoints.

To create your `secrets.yml` file, run:

```
cp ./config/secrets.yml.stub ./config/secrets.yml
```

Then open up `config/secrets.yml` and provide the values for `ALGOLIA_APP_ID` and `ALGOLIA_ADMIN_KEY` from your Algolia account.

Next, open up `config/indexes.js` and update the example index with your actual Algolia index information. E.g,

```
[
    {
        name: "dist",
        url: "https://example.com/index.json"
    }
]
```

## Usage

To deploy this function in development mode, run:

```
serverless deploy
```

To deploy this function with production secrets, run:

```
serverless deploy --stage prod --ALGOLIA_APP_ID=YOUR_APP_ID --ALGOLIA_ADMIN_KEY=YOUR_ADMIN_KEY
```

## Creating a schedule

By default, this function will run every hour. You can update this as desired by opening up `serverless.yml`, and changing `functions.main.events.schedule`.

## Triggering from a GitHub Webhook

When deploying the function, Serverless provides you with the Webtask endpoint that you can use to trigger the index update.

Head over to your GitHub repository, open up *Settings*, and then choose *Webhooks* from the sidebar.

1. Choose *Add Webhook*, and set the webtask URL provided by Serverless as the *Payload URL*.
2. Set the *Content Type* to *JSON*
3. Select *Let me select individual events*
4. Ensure only *Push* is selected
5. Ensure *Active* is checked
6. Click *Add webhook*

Now each time a change is pushed to your repository, this function will be triggered to deploy your Algolia index.

> **Note:** 
> Depending on how long your site takes to deploy, this may cause the index to be updated prematurely.
> To fix this, you can set the debounce variable in `secrets.yml` to make the function wait X number of seconds before running.
