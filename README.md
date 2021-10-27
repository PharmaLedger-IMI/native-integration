# Pharmaledger Native Integration

Provides Access to Native device features when available.
Means to provide a common interface for Native Features in most environments (browser, iOS, Android)

## Usage

By running the command ```npm run prebuild``` a bundle file will be produced per supported environment under:
 - ./build/bundles/default - default (browser) implementation;
 - ./build/bundles/ios - iOS implementation (expands the default functionality);
 - ./build/bundles/android - not developed;

The ENVIRONMENT specific file should be loaded in the loader's index.html file via:
```javascript
    <script src="camera.js"></script>
```

while the default one should be loaded the same way in the index.html of your application;

example octopus code to copy the right files:
```json
        {
            "name": "patch ios loader with ios native APIs",
            "actions": [
                {
                    "type": "copy",
                    "src": "native-integration/build/bundles/ios/camera.js",
                    "target": "mobile/scan-app/ios/PSKNodeServer/PSKNodeServer/nodejsProject/apihub-root/app/loader/camera.js",
                    "options": {
                        "overwrite": true
                    }
                }
            ]
        }
```

## Camera

Available for:
 - Browser (default): 
   - API: 
     - getCameraStream;
     - bindStreamToElement;
     - stopCameraStream;
 - iOS:
   - API:
     - getCameraStream;
     - bindStreamToElement;
     - stopCameraStream;
     - nativeBridge (provides access to all complex native Camera Apis, torch, focus, color space, etc)

### Dependencies

Needs to be part of a workspace with access to the PrivateSky/PSK-Release repositories

#### iOS:

needs carthage installed;

Depends on the repositories (should be included in the supporting workspace, for example view acdc-workspace):
 - https://github.com/PharmaLedger-IMI/acdc-ios-edge-agent.git
 - https://github.com/PharmaLedger-IMI/pharmaledger-camera.git

example of octopus code to handle these repositories;
```json
        {
            "name": "mobile/scan-app/ios",
            "src": "https://github.com/PharmaLedger-IMI/acdc-ios-edge-agent.git",
            "actions": [
                {
                    "type": "smartClone",
                    "target": ".",
                    "collectLog": false
                },
                {
                    "type": "execute",
                    "cmd": "echo \"Continue with ipa builds\""
                }
            ]
        },
        {
            "name": "mobile/scan-app/ios/pharmaledger-camera",
            "src": "https://github.com/PharmaLedger-IMI/pharmaledger-camera.git",
            "actions": [
                {
                    "type": "smartClone",
                    "target": ".",
                    "collectLog": false
                },
                {
                    "type": "execute",
                    "cmd": "echo \"Continue with ipa builds\""
                }
            ]
        },
        {
            "name": "update carthage",
            "src": ".",
            "actions": [
                {
                    "type": "execute",
                    "cmd": "cd mobile/scan-app/ios/PSKNodeServer/ && carthage update"
                },
                {
                    "type": "execute",
                    "cmd": "cd 'mobile/scan-app/ios/pharmaledger-camera/PharmaLedger Camera/' && ./carthage.sh"
                }
            ]
        }
```
