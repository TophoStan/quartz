Voor het ophalen van modules moet je gebruik maken van een service acount. Deze staat los van [[Service account]] `Hosting`. 

Om te begrijpen hoe de modules werken graag deze gids volgen
[Google example](https://developers.google.com/drive/api/quickstart/nodejs)


### Enable the API

Before using Google APIs, you need to turn them on in a Google Cloud project. You can turn on one or more APIs in a single Google Cloud project.

- In the Google Cloud console, enable the Google Drive API.
    
    [Enable the API](https://console.cloud.google.com/flows/enableapi?apiid=drive.googleapis.com)
    

### Configure the OAuth consent screen

If you're using a new Google Cloud project to complete this quickstart, configure the OAuth consent screen and add yourself as a test user. If you've already completed this step for your Cloud project, skip to the next section.

1. In the Google Cloud console, go to Menu menu > **APIs & Services** > **OAuth consent screen**.
    
    [Go to OAuth consent screen](https://console.cloud.google.com/apis/credentials/consent)
    
2. Select the user type for your app, then click **Create**.
3. Complete the app registration form, then click **Save and Continue**.
4. For now, you can skip adding scopes and click **Save and Continue**. In the future, when you create an app for use outside of your Google Workspace organization, you must add and verify the authorization scopes that your app requires.
    
5. If you selected **External** for user type, add test users:
    1. Under **Test users**, click **Add users**.
    2. Enter your email address and any other authorized test users, then click **Save and Continue**.
6. Review your app registration summary. To make changes, click **Edit**. If the app registration looks OK, click **Back to Dashboard**.

### Authorize credentials for a desktop application

To authenticate as an end user and access user data in your app, you need to create one or more OAuth 2.0 Client IDs. A client ID is used to identify a single app to Google's OAuth servers. If your app runs on multiple platforms, you must create a separate client ID for each platform.

**Caution:** This quickstart must be run locally and with access to a browser. It doesn't work if run on a remote terminal such as Cloud Shell or over SSH.

1. In the Google Cloud console, go to Menu menu > **APIs & Services** > **Credentials**.
    
    [Go to Credentials](https://console.cloud.google.com/apis/credentials)
    
2. Click **Create Credentials** > **OAuth client ID**.
3. Click **Application type** > **Desktop app**.
4. In the **Name** field, type a name for the credential. This name is only shown in the Google Cloud console.
5. Click **Create**. The OAuth client created screen appears, showing your new Client ID and Client secret.
6. Click **OK**. The newly created credential appears under **OAuth 2.0 Client IDs.**
7. Save the downloaded JSON file as `credentials.json`, and move the file to your working directory.