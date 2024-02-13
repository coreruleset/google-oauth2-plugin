# OWASP CRS - Google OAuth2 Plugin

## Description

Plugin to suppress false positives with Google OAuth2 online authorization
service callbacks.

## Installation

For full and up to date instructions for the different available plugin
installation methods, refer to [How to Install a Plugin](https://coreruleset.org/docs/concepts/plugins/#how-to-install-a-plugin)
in the official CRS documentation.

## Testing

After installation, plugin should be tested, for example, using these two commands:  
```
curl "http://localhost/?state=test&code=test&scope=email+profile+https://www.googleapis.com/auth/userinfo.profile+https://www.googleapis.com/auth/userinfo.email"
curl "http://localhost/?state=test&code=test&scope=email+profile+https://www.googleapis.com/auth/userinfo.profile+https://www.googleapis.com/auth/userinfo.email&inject=data"
```

Using default CRS configuration, first command should complete successfully
without anything logged into logs and second command should end with status 403
with the following message in the log:  
`ModSecurity: Warning. Matched phrase ".profile" at ARGS:scope. [file "/path/rules/REQUEST-930-APPLICATION-ATTACK-LFI.conf"] [line "97"] [id "930120"] [msg "OS File Access Attempt"] [data "Matched Data: .profile found within ARGS:scope: email profile https:/www.googleapis.com/auth/userinfo.profile https:/www.googleapis.com/auth/userinfo.email"] [severity "CRITICAL"] [ver "OWASP_CRS/3.3.2"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "attack-lfi"] [tag "paranoia-level/1"] [tag "OWASP_CRS"] [tag "capec/1000/255/153/126"] [tag "PCI/6.5.4"] [hostname "localhost"] [uri "/"] [unique_id "YfKalxtdAK3KYVNciYWEkgAAACs"]`

## License

Copyright (c) 2022 OWASP CRS project. All rights reserved.

The OWASP CRS and its official plugins are distributed
under Apache Software License (ASL) version 2. Please see the enclosed LICENSE
file for full details.
