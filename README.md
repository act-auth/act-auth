# Act Auth

# Introduction

Act Auth is an open protocol with granularity of action to allow for more controllable authorization.

## Origin
### Account granularity
Previously, the common authorization method was that the user provided the account name and password directly to the third party, and the third party logged in the service where the source data  located as the user, and then obtained the user's data to provide services for the user.

![](https://cdn.nlark.com/yuque/__puml/bf24cd36609e92c00ddfcea7f83d7c27.svg#card=puml&code=%40startuml%0A%0Aactor%20User%0Aparticipant%20%22Source%20Data%22%20as%20Source%0Aparticipant%20%22Third%20Party%22%20as%20Third%0A%0AUser%20-%3E%20Source%3A%20Create%20and%20save%20data%0A%3D%3D%3D%3D%0AUser%20-%3E%20Third%3A%20Provide%20account%20and%20password%0Aactivate%20User%0Aactivate%20Third%0AThird%20%3C-%3E%20Source%3A%20Log%20in%20as%20User%20and%20obtaine%20data%0AThird%20-%3E%20User%3A%20Provide%20services%20through%20data%0Adeactivate%20User%0Adeactivate%20Third%0A%0A%40enduml)
In this way:

```
AccountGranularity = User
```


After the appearance of OAuth, third parties began to use tokens to obtain authorized user data.

![](https://cdn.nlark.com/yuque/__puml/23627cc72e17683bdac4ddab32689110.svg#card=puml&code=%40startuml%0A%0Aactor%20User%0Aparticipant%20%22Source%20Data%22%20as%20Source%0Aparticipant%20%22Third%20Party%22%20as%20Third%0A%0AUser%20-%3E%20Source%3A%20Create%20and%20save%20data%0AThird%20-%3E%20Source%3A%20Fetch%20access%20token%0A%3D%3D%3D%3D%0AUser%20-%3E%20Source%3A%20Log%20in%20and%20authorize%0Aactivate%20User%0ASource%20-%3E%20Third%3A%20Callback%0Aactivate%20Third%0AThird%20%3C-%3E%20Source%3A%20Obtaine%20data%20with%20token%0AThird%20-%3E%20User%3A%20Provide%20services%20through%20data%0Adeactivate%20User%0Adeactivate%20Third%0A%0A%40enduml)
In this way:

```
AccountGranularity = User * ThirdParty
```


In Act Auth, account granularity is the action between user and third party.


---
# With
## GraphQL
TODO


## Serverless
TODO


## GDPR
TODO


---
# Contributing
ActAuth has been preparing since December 2017 and is currently drafting the first edition of the specification. If you want to learn more or join us, send email to work@imhele.com.
