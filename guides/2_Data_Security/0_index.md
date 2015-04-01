#Data Security

We encrypt all data sent from our API to your device (and vice-versa). We protect all access to the CloudMine API with SSL encryption over HTTPS.

We do not expose any insecure endpoints: all API calls must be made over SSL, which effectively eliminates the possibility of eavesdroppers reading your data as it is sent over the network.

#App-Level Security

Many apps need more security than hiding a secret key by embedding it in an app. CloudMine allows you to generate new API keys with restricted permissions, enabling you to distribute a key with your app that will enforce your access requirements on our servers. Keys can be managed through the Dashboard.

## Security Guidelines

We recommend creating a new API key for each independent client that needs to access your app's data. When deciding which permissions to grant on a key, remember to follow the [Principle of Least Privilege](http://en.wikipedia.org/wiki/Principle_of_least_privilege). Only grant access to data that is required for the functioning of the app. By following this guideline, you will ensure that if your API key is compromised, a malicious user will only be able to access data granted by the key.

An app is only as secure as its most permissive public API key. Keeping secret data inaccessible requires you to guard your sensitive API keys.
