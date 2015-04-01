# Welcome to CloudMine

CloudMine is a platform made up of a set of APIs and integration services that developers can use to enhance mobile apps with pre-made, standard backend services such as identity management, data storage, and push notifications. These services are accessed via REST APIs and client-side SDKs. The CloudMine backend is extendable via Java and JavaScript extensions, and can integrate where necessary with existing relational databases and many other enterprise applications.

This document is going to take you through the concepts CloudMine uses throughout. If you're looking for an orientation on the [CloudMine Dashboard](/dashboard), you should [go here](#/guides#dashboard) instead.

## Apps ##
CloudMine allows developers to create any number of Apps. Apps are the foundation of CloudMine and they encapsulate all data inside them. Every app is given a unique *Application Identifier*, shortened to *App ID* often. In all API Calls that interface with the App (all of them), the URL will include the *App ID*, such as:

```http
https://api.cloudmine.me/v1/app/appid/text
```

The `appid` in the URL above would be replaced by the App ID from the dashboard.

App ID’s are *not* authentication. App ID’s are considered public and are very easy to gain access to. To ensure your application is secure, **every** API request is required to have an API Key sent with it. This API Key is sent as a header or as a query parameter.

Since the API Key is being sent with the request, it too can be accessed easily. While your App ID will never change, changing the API Key is much easier. API Keys can be regenerated and changed if compromised.

Furthermore, API Keys have permissions on them, which can be dynamically changed from the dashboard. It’s important to give the API Key the most restrictive permissions possible. For example, if your App has some data accessible for all users of the app (App Level Data), then the API Key will need to have *read* permission. But it should not have *create* or *delete* privileges, as that could result in a malicious user deleting all the app data.

Lastly, API Key permissions only impact calls made to App Level data. As you’ll read below, users have their own personal data, and they *always* have full access to their own information.

You can read more about API Keys and securing your app in [API Key](#/data_security).

## Objects ##

CloudMine lets you store arbitrary pieces of data in our schemaless backend. This allows for a lot of flexibility between what you are storing and eliminates the need to do tedious migrations, versioning, and creating tables on the backend.

CloudMine objects are JSON objects - anything you can store in JSON can be sent to CloudMine. CloudMine expects a certain format for the objects that are being worked with.

For example, here is a CloudMine Object:

```json
"object-key": {
	"aKey": "a value",
	"anotherKey": 20
}
```

CloudMine Objects are just JSON, but they all have the same format of `key`:`{object}`. The key needs to be unique between objects, otherwise you will override the object that has that key.

Key's and Objects are the foundation of CloudMine's simple, but powerful structure. Every object has a `key` associated with it, and you use that key as the object's ID - that's how you find objects, update objects, and delete them.

When you are working directly with the REST API you need to ensure the objects are properly formatted, but the CloudMine SDK's should extrapolate most of this work away for you.

CloudMine can also store binary files, and this format is similar, but slightly different (since a majority of the data is binary). You can read more about files [here](#/rest_api#upload-new-or-replace-file).

## Users ##
CloudMine has first class user accounts, which allow you to create users for your applications in just a few lines of code. Users are created in one API Call, and then logged in in a separate API Call, but most of this is done by the SDK’s. User accounts also have a unique key that can be used to access them, although this isn’t used often.

Once the user has been logged in, in addition to sending the API Key with each request, every *user* request will need the users’s Session Token sent too.

Every user has a concept of owning his or her’s own objects. This brings us to a big distinction in where the data is saved.

### App Level vs User Level ###
There are two levels of data access in CloudMine. An object can be *App Level*, which means anyone with the App ID and API Key can access it. Then there is User level data, which means that only the logged in user can access it.

The most common use case is for App level data to be public information that all users should be able to access. The users then create their own personal information which is saved under each user, and is protected.

Keep in mind, API Keys only restrict App level calls, so users will always be able to login and modify their own information, *even if the API Key has no permissions*.

User objects can be made even more powerful through the use of Access Control Lists (ACL). ACLs are added to user objects with their own set of permissions. The ACL can specify which **other** users have permission to modify the user’s object. These permissions are CRUD operations, but also include *public*, allowing for non-logged in users to view the object.

You can read more about ACL’s in our [ACL Guide](#/rest_api#user-data-security).

## Server Code ##
Often an application will have to have custom logic run, and this logic is most appropriate on the server. CloudMine allows arbitrary Java and Javascript code snippets to be run on the CloudMine servers. These code snippets have super fast access to the main data store and should be used for complex queries and post processing, and server side logic (such as sending out push notifications). Snippets can also be scheduled and run at intervals.
