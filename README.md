# zego_permission

Goes along with the Zego Express Engine for Flutter (with Null Safety).

## Getting Started

Use this code to incorporate within your Zego project:

```dart
class Authorization {
  final bool camera;
  final bool microphone;

  Authorization(this.camera, this.microphone);
}

// Apply permission
Future<Authorization?> checkAuthorization() async {
  List<Permission>? statusList = await ZegoPermission.getPermissions(<PermissionType>[PermissionType.Camera, PermissionType.MicroPhone]);

  if (statusList == null) return null;

  PermissionStatus cameraStatus = PermissionStatus.unknown, micStatus = PermissionStatus.unknown;
  for (var permission in statusList) {
    if (permission.permissionType == PermissionType.Camera) cameraStatus = permission.permissionStatus;
    if (permission.permissionType == PermissionType.MicroPhone) micStatus = permission.permissionStatus;
  }

  bool camReqResult = true, micReqResult = true;
  if (cameraStatus != PermissionStatus.granted || micStatus != PermissionStatus.granted) {
    if (cameraStatus != PermissionStatus.granted) {
      camReqResult = await ZegoPermission.requestPermission(PermissionType.Camera) ?? true;
    }

    if (micStatus != PermissionStatus.granted) {
      micReqResult = await ZegoPermission.requestPermission(PermissionType.MicroPhone) ?? true;
    }
  }

  return Authorization(camReqResult, micReqResult);
}

Future<bool?> checkMicAuthorization() async {
  List<Permission>? statusList = await ZegoPermission.getPermissions(<PermissionType>[PermissionType.MicroPhone]);

  if (statusList == null) return null;

  PermissionStatus micStatus = PermissionStatus.unknown;
  for (var permission in statusList) {
    if (permission.permissionType == PermissionType.MicroPhone) micStatus = permission.permissionStatus;
  }

  bool micReqResult = true;
  if (micStatus != PermissionStatus.granted) {
    if (micStatus != PermissionStatus.granted) {
      micReqResult = await ZegoPermission.requestPermission(PermissionType.MicroPhone) ?? true;
    }
  }
```
  return micReqResult;
}
