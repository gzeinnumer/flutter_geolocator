# flutter_geolocator

|<img src="/preview/preview1.PNG" width="300"/>|<img src="/preview/preview2.jpeg" width="300"/>|
|--|--|


[Source](https://pub.dev/packages/geolocator)

```
#  todo 1
geolocator: ^8.2.0
geocoding: ^2.0.0
```

- main.dart
```dart
import 'package:geocoding/geocoding.dart';
import 'package:geolocator/geolocator.dart';

// todo 2

/*
ios/Runner/Info.plist
	<key>NSLocationWhenInUseUsageDescription</key>
	<string>This app needs access to location when open.</string>
	<key>NSLocationAlwaysUsageDescription</key>
	<string>This app needs access to location when in the background.</string>
 */
/*
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
 */

...
```
```dart
class _MyHomePageState extends State<MyHomePage> {
```
```dart
  //todo 3
  Position? _position;
  String? _address;
  void _getCurrentLocation() async {
    Position position = await _determinePosition();
    String address = await _getAddressFormLatLongOnline(position);
    setState(() {
      _position = position;
      _address = address;
    });
  }
```
```dart
  //todo 4
  Future<Position> _determinePosition() async {
    LocationPermission permission;
    permission = await Geolocator.checkPermission();
    if (permission == LocationPermission.denied) {
      permission = await Geolocator.requestPermission();
      if (permission == LocationPermission.denied) {
        // Permissions are denied, next time you could try
        // requesting permissions again (this is also where
        // Android's shouldShowRequestPermissionRationale
        // returned true. According to Android guidelines
        // your App should show an explanatory UI now.
        return Future.error('Location permissions are denied');
      }
    }
    if (permission == LocationPermission.deniedForever) {
      // Permissions are denied forever, handle appropriately.
      return Future.error(
          'Location permissions are permanently denied, we cannot request permissions.');
    }
// When we reach here, permissions are granted and we can
    // continue accessing the position of the device.
    return await Geolocator.getCurrentPosition();
  }
```
```dart
  //todo 5
  Future<String> _getAddressFormLatLongOnline(Position param) async {
    try {
      List<Placemark> placemarkList = await placemarkFromCoordinates(
        param.latitude,
        param.longitude,
        localeIdentifier: "id",
      );
      Placemark place = placemarkList[0];
      return "${place.country} ${place.postalCode} ${place.administrativeArea} ${place.subAdministrativeArea} ${place.locality} ${place.subLocality} ${place.thoroughfare} ${place.subThoroughfare} ${place.name}";
    } catch (e) {
      return "Log error$e";
    }
  }
```
```dart
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Geolocation Example'),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: [
              //todo 7 result
              _position != null ? Text('Current Location: $_position'): Text('No location data'),
              _address != null ? Text('Current Address: $_address'): Text('No address data'),
            ],
          ),
        ),
        floatingActionButton: FloatingActionButton(
            //todo 6 use function
          onPressed: _getCurrentLocation,
          tooltip: 'Increment',
          child: const Icon(Icons.add),
        )
    );
  }
}
```

---

```
Copyright 2022 M. Fadli Zein
```