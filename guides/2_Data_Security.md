# API Key Examples

### Read-only key

The easiest way to secure app-level data with CloudMine is to make all data read-only. Create a new key with the following selections for the "All Data" and "All Files" rules (note that the green outlines indicate that the permission is granted):

{{img "applevel-readonly.png"}}

This key can then be safely embedded in your app, since it will only allow reading data.

### Multiple keys

Some apps may have multiple clients which need to access the data with different permission levels. You can use the Dashboard to add as many keys as you need for your app.

For this example, we'll create two API keys, one for each client. The "All Access" key has access to all data.

{{img "applevel-fullaccess.png"}}

This will allow users of the administrator client to alter any app-level data.

For the second key, we'll assume that we want to prevent users from reading any object that has a property `class` with the value `admin`.

{{img "applevel-consumer.png"}}

### Multiple rules

Each key can have many rules, which are combined when requests are made. For example, this key would allow users to read objects where `name` is equal to `read`, and update objects where `name` is equal to `update`.

**This key does not permit deleting or creating any objects.**

{{img "applevel-multiplerule.png"}}
