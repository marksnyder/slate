---
title: Chronicity.IO

language_tabs: # must be one of https://git.io/vQNgJ
  - csharp

toc_footers:
  - <a href='https://github.com/marksnyder/Chronicity'>Source Code</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:


search: true
---

# Introduction

Chronicity can be leveraged directly in .NET Core or via web service.


# Initializing

In order to use the timeline service you must create an instance of ITimeLineService.


```csharp
var service = new TimeLineService();
```

# Events

Events can be tracked and linked to one or more entities.

## Register New Event

```csharp
var e1 = new NewEvent()
{
    On = "2001/01/01 01:01",
    Type = "MyEventType",
    Entities = new [] { "MyEntity" }
};

service.RegisterEvent(e1);

```

# Observations

Observations directly impact entity state.

## Register New Observation

```csharp
var o1 = new Observation()
{
   On = "2001/01/01 01:01",
   Entity = "MyEntity",
   Expressions = new[] { "State.MyVal=Hello World" }
};

service.RegisterObservation(o1);

```

## Filtering Events


> Event Filter

```csharp
service.FilterEvents(new string[] { "State.MyVal = Hello World" });

service.FilterEvents(new string[] { "On > 2001/01/01 01:01" });

service.FilterEvents(new string[] { "On < 2001/01/01 01:02" });

service.FilterEvents(new string[] { "On <= 2001/01/01 01:00", "On >= 2001/01/01 01:02" });

service.FilterEvents(new string[] { "Type = MyEventType" });

service.FilterEvents(new string[] { "Type = [MyEventType1,MyEventType2]" });

```


Response: (Array)

Property  | Description
--------- | ---------
Type | Type of event
On | Date and time of event
Entities | List of related entities & state
Id | Unique identifier


# Entity State

Over time entity state may change (via observations).

## Retrieving Entity State (Given Time)

```csharp
service.GetEntityState("MyEntity","2001/01/01 01:01");
```

## Retrieving State Ranges (Value Comparisons)

```csharp
service.FilterState(new[] { "State.MyVal=Hello World" })

service.FilterState(new[] { "State.MyNumericVal <= 10" });

service.FilterState(new[] { "State.MyNumericVal = 9" });

service.FilterState(new[] { "State.MyNumericVal > 14" });

```

Response: (Array)

Property  | Description
--------- | ---------
Entity | Owner of state
Start | Beginning of state range
End | End of state range (null if current)
Key | State key
Value | State value

# Event Cluster

## Retrieving Event Clusters

```csharp

_service.SearchClusters(
                new [] { "On < 2001/01/01 01:00" },
                new [] { "Within <= 0.0:5:0" });


_service.ClusterEvents(
                new string[] { "On > 2001/01/01 01:00" },
                new string[] {
                    " 1 | MyCluster1 | Sequence = [MyEventType1,MyEventType2]",
                    " 1 | MyCluster2 | Sequence = [MyEventType3,MyEventType4]",
                    " 2 | FinalCluster | Sequence = [MyCluster1,MyCluster2]"
                });

```


Response: (Array)

Property  | Description
--------- | ---------
Entities (Array) | Related entities
Start | Beginning of cluster
End | End of cluster
Events | (Array) Events contained within cluster
