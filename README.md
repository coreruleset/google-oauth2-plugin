# OWASP ModSecurity Core Rule Set - Google OAuth2 Plugin

## Description

Plugin to suppress false positives with Google OAuth2 online authorization
service callbacks.

## Prerequisities

 * CRS version 3.4 or newer (or see "Preparation for older installations" below)

## Plugin installation

Copy all files from `plugins` directory into the `plugins` directory of your
OWASP ModSecurity Core Rule Set (CRS) installation.

### Preparation for older installations

 * Create a folder named `plugins` in your existing CRS installation. That
   folder is meant to be on the same level as the `rules` folder. So there is
   your `crs-setup.conf` file and next to it the two folders `rules` and
   `plugins`.
 * Update your CRS rules include to follow this pattern:

```
<IfModule security2_module>

 Include modsecurity.d/owasp-modsecurity-crs/crs-setup.conf

 Include modsecurity.d/owasp-modsecurity-crs/plugins/*-config.conf
 Include modsecurity.d/owasp-modsecurity-crs/plugins/*-before.conf
 Include modsecurity.d/owasp-modsecurity-crs/rules/*.conf
 Include modsecurity.d/owasp-modsecurity-crs/plugins/*-after.conf

</IfModule>
```

_Your exact config may look a bit different, namely the paths. The important
part is to accompany the rules-include with two plugins-includes before and
after like above. Adjust the paths accordingly._

## Testing

After installation, plugin should be tested, for example, using these two commands:  
`curl "http://localhost/?state=test&code=test&scope=email+profile+https://www.googleapis.com/auth/userinfo.profile+https://www.googleapis.com/auth/userinfo.email"`
`curl "http://localhost/?state=test&code=test&scope=email+profile+https://www.googleapis.com/auth/userinfo.profile+https://www.googleapis.com/auth/userinfo.email&inject=data"`

Using default CRS configuration, first command should complete successfully
without anything logged into logs and second command should end with status 403
with the following message in the log:  
`ModSecurity: Warning. Matched phrase ".profile" at ARGS:scope. [file "/path/rules/REQUEST-930-APPLICATION-ATTACK-LFI.conf"] [line "97"] [id "930120"] [msg "OS File Access Attempt"] [data "Matched Data: .profile found within ARGS:scope: email profile https:/www.googleapis.com/auth/userinfo.profile https:/www.googleapis.com/auth/userinfo.email"] [severity "CRITICAL"] [ver "OWASP_CRS/3.3.2"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "attack-lfi"] [tag "paranoia-level/1"] [tag "OWASP_CRS"] [tag "capec/1000/255/153/126"] [tag "PCI/6.5.4"] [hostname "localhost"] [uri "/"] [unique_id "YfKalxtdAK3KYVNciYWEkgAAACs"]`

## License

Copyright (c) 2022 OWASP ModSecurity Core Rule Set project. All rights reserved.

The OWASP ModSecurity Core Rule Set and its official plugins are distributed
under Apache Software License (ASL) version 2. Please see the enclosed LICENSE
file for full details.
