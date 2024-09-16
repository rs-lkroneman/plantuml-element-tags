# PlantUML Element Tags

## ElementTags

mobileApplication - purple
webApplication - green
adminApplication - blue
internalService - aqua
awsService - orange
hiddenContainer - transparent

## Rels (Lines between things)

mobileRel - purple
webRel - green
adminRel - blue
internalRel - aqua
externalRel - red

## Example

```plantuml
@startuml examples/basic-c4-diagram

!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
!include c4-theme.puml

title E$ Customer Engagement API - C2 Diagram

System_Ext(people, "People", $tags="hiddenContainer") {
  Person(user, "User")
  Person(teamMember, "Team Member")
}

System_Boundary(mainSystem, "Main Internal System") {
    System(clients, "Clients", $tags="hiddenContainer") {
      Container(mobileClient, "Mobile Client", $tags="mobileApplication")
      Container(webClient, "Web Client", "Vue/Java", $tags="webApplication")

      Lay_R(webClient, mobileClient)
    }

    System(services, "Services", $tags="hiddenContainer") {
      Container(customerEngagementPlatform, "E$ Customer Engagement API", "Spring Boot/Java", "Provides user-specific information for use in dynamic messaging", $tags="internalService")
      Container(mainApi, "Main API", "", "Provides information", $tags="internalService")

      Lay_R(customerEngagementPlatform, mainApi)
    }

    Lay_D(webClient, mainApi)
    Lay_D(mobileClient, customerEngagementPlatform)
}

System_Ext(externalServices, "External Services", $tags="hiddenContainer") {
    System_Ext(braze, "Customer engagement platform")
    System_Ext(logger, "Logging Service")
    System_Ext(sentry, "Monitoring Service")

    Lay_R(braze, logger)
    Lay_R(logger, sentry)
}

Rel(user, mobileClient, "Uses")
Rel(user, webClient, "Uses")
Rel(teamMember, braze, "Uses")
Rel(mobileClient, braze, "Registers for push notifications, in-app messages, and content cards", $tags="mobileRel")
Rel(webClient, braze, "Registers for content cards", $tags="webRel")
Rel(braze, customerEngagementPlatform, "Requests user info")
Rel(customerEngagementPlatform, mainApi, "Requests user-specific budget info")
Rel(customerEngagementPlatform, logger, "Logs events during service processing of each request")
Rel(customerEngagementPlatform, sentry, "Sends monitoring data")

SHOW_LEGEND()

```
