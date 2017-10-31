- https://docs.expo.io/versions/latest/guides/configuration.html

Sample:  
```
{
  "expo": {
    "name": "My app",
    "slug": "my-app",
    "sdkVersion": "21.0.0",
    "privacy": "public"
  }
}
```

Most configuration from app.json is accessible at runtime from your JavaScript code via `Expo.Constants.manifest`.   

Properties:
- name*
- description
- slug*  
The friendly url name for publishing. eg: expo.o/@your-username/slug.
- privacy  
`public` or `unlisted`
- sdkVersion*  
The Expo sdkVersion to run the project on.
- version
- orientation  
Lock your app to a specific orientation with `portrait` or `landscape`. Defaults to no lock. Valid values: `‘default’, ‘portrait’, ‘landscape’`  
- primaryColor
- icon  
- notification  
Configuration for remote (push) notifications.
  - icon
  - color
  - androidMode  
  Valid values: `‘default’, ‘collapse’`
  - androidCollapsedTitle
- loading  
Configuration for the loading screen that users see when opening your app, while fetching & caching bundle and assets.
  - icon
  - exponentIconColor
  - exponentIconGrayscale
  - backgroundImage
  - backgroundColor
  - hideExponentText
- appKey
- androidStatusBarColor
- androidStatusBar
  - barStyle
  - backgroundColor
- androidShowExponentNotificationInShellApp
- scheme
- entryPoint  
The relative path to your main JavaScript file.
- extra  
Any extra fields you want to pass to your experience. Values are accessible via `Expo.Constants.manifest.extra`
- rnCliPath
- packagerOpts
- ignoreNodeModulesValidation
- nodeModulesPath
- ios  
Standalone Apps Only. iOS standalone app specific configuration  
  - bundleIdentifier
  - buildNumber
  - icon
  - merchantId  
  Merchant ID for use with Apple Pay in your standalone app.  
  - config
    - branch
      - apiKey
    - usesNonExemptEncryption
    - googleMapsApiKey
    - googleSignIn
      - reservedClientId
  - isRemoteJSEnabled
  - supportsTablet
  - isTabletOnly
  - infoPlist
  - associatedDomains
  - usesIcloudStorage
  - splash
    - backgroundColor
    - resizeMode
    - image
    - tabletImage
- android
  - package
  - versionCode
  - icon
  - permissions
  - config
    - branch
      - apiKey
    - fabric
      - apiKey
      - buildSecret
    - googleMaps
      - apiKey
    - googleSignIn
      - apiKey
      - certificateHash
- splash
  - ...
- facebookScheme
- hooks
  - postPublish
