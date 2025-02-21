---
title: Setting Up Google Authentication
---

The Google Authentication API lets you delegate authentication services to Google. To use this API, you first need to register your application. In return you will get a **Client ID** and a **Client Secret** with which to identify your application to Google and enable your application to receive information from their servers.

Google changes things all too frequently. The following steps are correct as of 2025 but they may change a bit. If so, just poke around and you'll find the right buttons and links.

1. Log in to Google's [Developer Console](https://console.cloud.google.com/welcome).
2. Click on **Create or select a project**. The link is right under the "Welcome" heading.
3. In the upper-right corner of the popup, click `New Project`.
    - Name your project; use something appropriate (like `ITC 210 Google Auth`).
    - It will generate a project ID for you. You can change the ID if you like.
    - Leave **Location** set to `No organization` if you are using a typical Google account. In some cases your account may belong to an organization in which case you may set the location to that organization.
    - Click **Create**.
    - The project is created and it returns you to the Google Cloud welcome page.
4. Next, you need to provide information for the OAuth consent screen. This is the screen that is presented to users when they log into your app for the first time.\
    - Browse to Google's [OAuth Overview](https://console.cloud.google.com/auth/overview)
    - The screen should say, "Google Auth Platform not configured yet" and your project should be listed in the upper-left next to "Google Cloud". Click there to select your new project if it is not already selected.
    - Now click on the blue **Get Started** button to configure.
    - Set an **App Name**. This is the display name that will show on the consent page. It can be the same as the project name or something different.
    - Set the **User support email** to your email address.
    - Click **Next**.
    - Set **Audience** to `External`.
    - Click **Next**.
    - Set **Contact Information** to your email address.
    - Click **Next**.
    - Accept the terms and services.
    - Click **Continue** and **Create**.
    - If you want to change any of these settings, you can select **Branding** in the left menu.
6. Next, set up your OAuth client.
    - Click on **Clients** in the left menu or browse to [OAuth Clients](https://console.cloud.google.com/auth/clients).
    - Sometimes it says, "To create an OAuth client ID, you must first configure your consent screen." If so, it is a bug, you just configured your consent screen. Just refresh the page and the message should go away.
    - Click on **+ Create Client** toward the top of the screen.
    - Set **Application type** to `Web Application`.
    - Set **Name** to whatever you want or just leave it as `Web client 1`.
    - Leave **Authorized JavaScript origins** with no configured URI values.
    - Under **Authorized Redirect URIs** click **+ Add URI**.
    - Set the new **URI** to `http://localhost:1337/api/v1/auth/google/callback`. This value will be used for local testing. Later, you will add a callback address for your live server.
    - Click **Create** at the bottom of the screen.
    - Your new client appears in the list of **OAuth 2.0 Client IDs**
    - Click on your new client.
    - To the right-hand side you have a **Client ID** and a **Client secret** each with a creation date. You will need these values later.
    
    Your google authentication is set up. Later, you will enter the **Client ID** and **Client secret** into your **.env** file so that your application has access to the values. Because these are "secrets", make sure they do not get committed to your repo. If you don't want to save them now, you can always browse back to the [OAuth Clients](https://console.cloud.google.com/auth/clients) page and retrieve them.
