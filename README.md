# WebRTC Hello World Video PSTN Call
<a href="http://dev.bandwidth.com"><img src="https://s3.amazonaws.com/bwdemos/BW-VMP.png"/></a>

 # Table of Contents

<!-- TOC -->

* [WebRTC Hello World Video PSTN Call](#webrtc-hello-world-video-pstn-call)
* [Description](#description)
* [Pre-Requisites](#pre-requisites)
* [Environmental Variables](#environmental-variables)
* [Callback URLs](#callback-urls)
* [Running the Application](#running-the-application)
  * [Run](#run)
  * [Ngrok](#ngrok)

<!-- /TOC -->

# Description

This sample app shows a very simple video to video or video to PSTN call. This is limited to 1:1 based on the very simple user management and UI, but extending it shouldn't be very difficult.

## Architecture Overview

<img src="./WebRTC Hello World.svg">

This app runs an HTTP server that listens for requests from browsers to get connection information. This connection information tells a browser the unique ID it should use to join a WebRTC conference. The HTTP server will also handle requests coming from Bandwidth's Voice API when a phone call comes in.

The server connects to Bandwidth's HTTP WebRTC API, which it will use to create a session and participant IDs. This example leverages our Node SDK to make the WebRTC and Voice API calls.

The web browser will also use a websocket managed by the WebRTC browser SDK to handle signaling to the WebRTC API - this is all handled by a prepackaged Javascript SDK. Once both a browser and a phone have joined the conference, they will be able to talk to each other.

> Note: This is a very simplistic demo and it is not designed to handle more than 2 browsers or multiple phones.<br/> Unless you are running on `localhost`, you will need to use HTTPS. Most modern browsers require a secure context when accessing cameras and microphones.

## Setting things up

To run this sample, you'll need a Bandwidth phone number, Voice API credentials and WebRTC enabled for your account. Please check with your account manager to ensure you are provisioned for WebRTC.

This sample will need be publicly accessible to the internet in order for Bandwidth API callbacks to work properly. Otherwise you'll need a tool like [ngrok](https://ngrok.com) to provide access from Bandwidth API callbacks to localhost.

### Create a Bandwidth Voice API application

Follow the steps in [How to Create a Voice API Application](https://support.bandwidth.com/hc/en-us/articles/360035060934-How-to-Create-a-Voice-API-Application-V2-) to create your Voice API appliation.

In step 7 and 8, make sure they are set to POST.

In step 9, provide the publicly accessible URL of your sample app. You need to add `/incomingCall` to the end of this URL in the Voice Application settings.

You do no need to set a callback user id or password.

Create the application and make note of your _Application ID_. You will provide this in the settings below.

### Configure your sample app

Copy the default configuration files

```bash
cp .env.default .env
```

Add your Bandwidth account settings to `.env`:

- BW_ACCOUNT_ID
- BW_USERNAME
- BW_PASSWORD

Add your Voice API application information:

- BW_VOICE_APPLICATION_ID

Enter your local server address (e.g. ngrok url):

- BASE_CALLBACK_URL

To make an outbound call from the browser, add a phone number to dial:

- USER_NUMBER
- BW_NUMBER (the number that will appear as the FROM for the call)

You can ignore any other settings in the `.env.default` file.

# Pre-Requisites

In order to use the Bandwidth API, users need to set up the appropriate application in their [Bandwidth Dashboard](https://dashboard.bandwidth.com/) and create [API credentials](https://dev.bandwidth.com/guides/accountCredentials.html#top).

To create an application, log into the [Bandwidth Dashboard](https://dashboard.bandwidth.com/) and navigate to the `Applications` tab.  Fill out the **New Application** form, selecting the service (Messaging or Voice) that the application will be used for.  All Bandwidth services require publicly accessible Callback URLs. For more information on how to set one up see [Callback URLs](#callback-urls).

For more information about API credentials see [here](https://dev.bandwidth.com/guides/accountCredentials.html#top).

# Environmental Variables

The sample app uses the below environmental variables.
```sh
BW_ACCOUNT_ID                        # Your Bandwidth Account Id
BW_USERNAME                          # Your Bandwidth API Username
BW_PASSWORD                          # Your Bandwidth API Password
BW_VOICE_APPLICATION_ID              # Your Voice Application Id created in the dashboard
BW_NUMBER                            # The Bandwidth phone number involved with this application
USER_NUMBER                          # The user's phone number involved with this application
BASE_CALLBACK_URL                    # Your public base url to receive Bandwidth Webhooks. No trailing '/'
LOCAL_PORT                           # The port number you wish to run the sample on
```

# Callback URLs

For a detailed introduction to Bandwidth Callbacks see https://dev.bandwidth.com/guides/callbacks/callbacks.html

Below are the callback paths:
* `/incomingCall`

# Running the Application
## Run
Use the following commands to run the application:

```sh
npm install
node server.js
```

## Ngrok

A simple way to set up a local callback URL for testing is to use the free tool [ngrok](https://ngrok.com/).  
After you have downloaded and installed `ngrok` run the following command to open a public tunnel to your port (`$LOCAL_PORT`)
```sh
ngrok http $LOCAL_PORT
```
You can view your public URL at `http://127.0.0.1:{LOCAL_PORT}` after ngrok is running.  You can also view the status of the tunnel and requests/responses here.
