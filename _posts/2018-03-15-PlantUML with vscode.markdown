---
layout:       post
title:        "PlantUML with VSCode"
subtitle:     "PlantUML with VSCode"
date:         2018-03-15 12:00:00
author:       "Gary"
header-img:   "img/post-bg-ubuntu.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - General
    - PlantUML
---

### PlantUML with VSCode
PlantUML is a component that allows to quickly write:
- Sequence diagram
- Usecase diagram
- Class diagram
- Activity diagram
- Component diagram
- State diagram
- Object diagram
- Deployment diagram 
- Timing diagram 

With VSCode, it can be even more powerful. VSCode has a PlantUML extension supporting review, export for multiple file-formats(png, svg etc.) 
### Get Started
##### Prerequisite
- Java
- [Graphviz](http://www.graphviz.org/download/)

for Graphviz, you need to add its dot.exe into environment variables

| Environment Variable Name        | Value                            |  
| ----------------------------     |:--------------------:            |  
| GRAPHVIZ_DOT                     | C:\Software\Graphviz\bin\dot.exe |


##### How to install
launch VSCode Quick Open(Ctrl + P), paste the following command, and press enter.
```bash
ext install plantuml
```

##### Basic Shourtcut

| Short Cut        | Opeartion   |  
| -------------    |:-----------:|  
| Alt + D          | Preview     |  
| Ctrl + Shift + P | Export      |
| Alt  + Shift + F | Auto Format |


##### Example
```xml
@startuml Encrypt
{
    styles: blue
}

package Int_LogViewerLib {
    class AccessLogViewerViewModel {

    }
}

package  Infrastructure.LoggingUtils.Xml {
     AccessLogViewerViewModel --> LogWatcher

    class LogEncryptor {
        + string EncryptData(string data)
        + string DecryptData(string data)
    }

    class EncryptLayoutRendererWrapper {
        + bool Encrypt
        # override string Transform(string text)
    }

    class LogWatcher {
        - bool performRead()
    }

    LogWatcher --> LogEncryptor: decrpyt data
    EncryptLayoutRendererWrapper --> LogEncryptor: encrypt data
}

package  NLog {
    EncryptLayoutRendererWrapper --|> WrapperLayoutRendererBase
}
@enduml
```

![Encrypt](http://www.plantuml.com/plantuml/png/XPB1JiCm38RlbVeEaRWCeUq1E4m3ZGDIuJ01uuQyr2iLRLCvBb11tnrtJKgxZP53b3Y_lz-sih0Cn5MUgPj2Krro68H12VeD4bIt1Rz49dbAyGCfIZVgAmoKB9s1jUfAQxIsVs_tEnDZrgewNHC6pP0dm2s1PQeqLuGpoKtI8ddaDBEpvppp_B_Hq-bSINWDh5-Hl4zNZyHT3uPwagmB9OvYupjS0iF4XM8vdn-HBl6Kj1aREsu4jQbuXKMCXBGcA4lSAsHZp63GFdwcp3iIfJ5w-mU2mcuDvTwSyPEFc_zEOsWrxQoaW9QoGeWd--c3H3VLVdgNAfR-ovftJNVoaqqU0h0xEi3u5zoBr1Vx3_PxhjnbhBW6YpliGWBoUty0 "Encrypt")