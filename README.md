## Laravel Echo for Flutter

Basically, this package is port of official [Laravel Echo javascript library](https://github.com/laravel/echo). It helps subscribe to channels and listen for events broadcasted from your Laravel app.

API is same as official Echo package, so everything in [Official documentation](https://laravel.com/docs/5.7/broadcasting) should work.

Three connectors available: [socket.io](#socket.io), [Pusher](#pusher), [Null](#null).

I use it with [laravel-websockets](https://github.com/beyondcode/laravel-websockets) server, not testing with official pusher server or socket.io 

## Usage

To use with __Pusher__, you need to clone my [pusher_dart](https://github.com/ed-lot/pusher_dart) client and link it in you Flutter app.

In your `pubspec.yaml` file:

```yaml
dependencies:
  ...
  pusher_dart:
      path: ../plugins/pusher_dart
  laravel_echo:
      path: ../plugins/echo
```

Init code exemple :

```dart
    import 'package:pusher_dart/Pusher.dart';
    //
    Pusher pusher = Pusher(
      "YOUR_PUSHER_APP_KEY",
      PusherOptions(
        host: "YOUR_DOMAIN",
        port: "YOUR_PORT",
        authEndpoint: "YOUR_AUTH_ENDPOINT",
        cluster: "YOUR_PUSHER_CLUSTER",
        encrypted: false,
        auth: PusherAuth(headers: {
        "Content-Type": "application/json",
        "authorization": "Bearer token",
        }),
        autoConnect: false,
        pingInterval: Duration(seconds: 5)),
    );

    Echo echo = new Echo({'broadcaster': 'pusher', 'client': pusher});
    echo.connect();
    echo.connecting((_) {
      // Connecting !
    });
    echo.connected((_) {
      // Connected !
    });
    echo.disconnected((_) {
      // Disconnected !
    });
```
Exemple of presence channel :
```dart
    echo.join('presence.1').here((dynamicMembers) {
      List<Member> members = List<Member>.from(dynamicMembers);
      // Users connected
    }).joining((dynamicMember) {
      Member member = dynamicMember;
      // New user connected 
    }).leaving((dynamicMember) {
      Member member = dynamicMember;
      // User disconnected
    });
```

Exemple of private channel (with notification) :
```dart
  echo.private("App.User.1").notification((notificationJson) {
    // Your code
  });
```
