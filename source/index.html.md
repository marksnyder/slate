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
   Expressions = new[] { "Entity.State.MyVal=Hello World" }
};

service.RegisterObservation(o1);

```

## Filtering Events


> Event Filter

```csharp
service.FilterEvents(new string[] { "Entity.State.MyVal=Hello World" });

service.FilterEvents(new string[] { "On.After=2001/01/01 01:01" });

service.FilterEvents(new string[] { "On.Before=2001/01/01 01:02" });

service.FilterEvents(new string[] { "On.Between=2001/01/01 01:00,2001/01/01 01:02" });

service.FilterEvents(new string[] { "Type=MyEventType" });

service.FilterEvents(new string[] { "Type=[MyEventType1,MyEventType2]" });

```


Response:

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
service.GetEntityState("MyEntity","2001 /01/01 01:01");
```

## Retrieving State Ranges (Value Comparisons)

```csharp
service.FilterState(new[] { "Entity.State.MyVal=Hello World" })

service.FilterState(new[] { "Entity.State.MyNumericVal <= 10" });

service.FilterState(new[] { "Entity.State.MyNumericVal = 9" });

service.FilterState(new[] { "Entity.State.MyNumericVal > 14" });

```


# Event Cluster

## Retrieving Event Clusters

```csharp

_service.SearchClusters(
                new [] { "On.After=2001/01/01 01:00" },
                new [] { "TimeSpan <= 0.0:5:0" });

```
