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

Chronicity seeks to provide a data store that is time and state centric. As opposed to traditional relational database systems which focus on data integrity, Chronicity focuses on maintaining the state of a data model (and it's relationship to other models) over time.

This method of storage and retrieval will help data consumers solve analytical problems with less effort when time and context are key components.

Links:

Source Code:
<a href='https://github.com/marksnyder/Chronicity'>Github</a>

Nuget Packages:
<a href='https://www.nuget.org/packages/Chronicity.Core/'>Chronicity Core</a>  |  
<a href='https://www.nuget.org/packages/Chronicity.Provider.InMemory/'>Chronicity.Provider.InMemory</a>

# Initializing

In order to use the timeline service you must create an instance of ITimeLineService.


> At this time an in memory provider is available. Future development will include persistent storage providers.

```csharp
var service = new Chronicity.Provider.InMemory.TimeLineService();
```

# Entities

Entities represent tracked state over time.

## Register Entity Type

Registering an entity adds it to the timeline and describes its type. Typing can be used later for filtering and also provides a namespace for the entity identifier.

```csharp
 service.RegisterEntity("MyEntityID", "MyEntityType");
```

# Events

Events can modify the state and relationships of registered entities.

## Register Events

```csharp
var e = new Event()
 {
     On = "2001/01/01 01:01",
     Type = "MyEventType",
     Entity = "MyEntityID",
     Observations = new string[] { "Entity.State.MyVal=Hello World" }
 };

service.RegisterEvent(e);

```

## Filtering Events

> By Type

```csharp
service.FilterEvents(new string[] { "Type=MyEventType" });
```

> By Time (Before)

```csharp
service.FilterEvents(new string[] { "On.Before=2001/01/01 01:02" });
```

> By Time (After)

```csharp
service.FilterEvents(new string[] { "On.After=2001/01/01 01:02" });
```

> By Time (Between)

```csharp
service.FilterEvents(new string[] { "On.Between=2001/01/01 01:00,2001/01/01 01:02" });
```

> By Entity State

```csharp
service.FilterEvents(new string[] { "Entity.State.MyVal=Hello World" });
```

> By Entity Type

```csharp
service.FilterEvents(new string[] { "Entity.Type=MyEntityType" });
```

Registered events can be searched using expression filters. Filters will return a results that contains:

Property  | Description
--------- | ---------
State | The state of the entity at the time the event was registered
Event | The matching registered event
