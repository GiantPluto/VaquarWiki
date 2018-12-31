## Google firebase  vs Realm  :

#### firebase :
- keepSynced(true) effectively keeps a listener active on the given reference, from the moment it's called, for as long as the app is running, so that the local version of the data is always in sync with the remote version on the server.

- setPersistenceEnabled(true) just activates local caching of the data read by the SDK. When persistence is enabled, the app can still query data previously read. It takes effect for all data read by the SDK. While persistence is enabled, you can't control which data is cached - all read data is cached up to 10MB max. When the max is reached the oldest data will be evicted from the cache.

- If you are using FirebaseDatabase.getInstance().setPersistenceEnabled(true); means that Firebase will create a local copy of your database which also means that every change that is made while you are offline, will be added to a queue. So, as this queue grows, local operations and application startup will slow down. So the speed depends on the dimension of that queue. But rememeber, Firebase is designed as an online database that can work for short to intermediate periods of being disconnected and not as an offline database.



- Firebase Realtime database is a remote database for storing the entire application’s data while realm is mostly used for localdata where you say one person’s data in phone and make it work even when the app is offline.
