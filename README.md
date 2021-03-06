# FriendlyChat

This repository contains code for the FriendlyChat project in the [Firebase in a Weekend: Android by Google](https://www.udacity.com/course/firebase-in-a-weekend-by-google-android--ud0352) Udacity course.

## Overview

FriendlyChat is an app that allows users to send and receive text and photos in real-time across platforms.

Used Firebase Services:
* Database
* Authentication
* Storage
* Analytics
* Cloud Messaging
* RemoteConfig

## Setup

Setup requires creating a Firebase project. See https://firebase.google.com/ for more information.

### Rules
#### Database Rules
For testing purposes only the DB setup has to be done in the Firebase Developer Console.
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

##### Basic rules with authentication
```json
{
  // https://console.firebase.google.com/project/friendlychat/database/friendlychat/rules
  "rules": {
    "messages": {
      // only authenticated users can read and write the messages node
    	".read": "auth != null",
    	".write": "auth != null",
      "$id": {
      	// the read and write rules cascade to the individual messages
       	// messages should have a 'name' and 'text' key or a 'name' and 'photoUrl' key
       	".validate": "newData.hasChildren(['name', 'text']) && !newData.hasChildren(['photoUrl']) || newData.hasChildren(['name', 'photoUrl']) && !newData.hasChildren(['text'])"
      }
    }
  }
}
```

#### Storage Rules
```json
// https://console.firebase.google.com/project/friendlychat-28cc5/storage/friendlychat-28cc5.appspot.com/rules
service firebase.storage {
  match /b/{bucket}/o {
    match /chat_photos/{allPaths=**} {
      allow read, write: if request.auth != null && request.resource.size < 3*1024*1024;
    }
  }
}
```

## License
See [LICENSE](LICENSE)
