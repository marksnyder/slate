---
title: Chronicity.IO

language_tabs: # must be one of https://git.io/vQNgJ
  - csharp

toc_footers:
  - <a href='https://github.com/marksnyder/Chronicity'>Source Code</a>

includes:
  - errors

search: true
---

# Introduction

Chronicity seeks to provide a data store that is time and state centric. As opposed to traditional relational database systems which focus on data integrity, Chronicity focuses on maintaining the state of a data model (and it's relationship to other models) over time.

This method of storage and retrieval will help data consumers solve analytical problems with less effort when time and context are key components.

# Initializing

> At this time an in memory provider is available. Future development will include persistent storage providers.

```csharp
var service = new Chronicity.Provider.InMemory.TimeLineService();
```

# Entities

Entities represent tracked state over time.

## Register Entity Type

```csharp
 service.RegisterEntity("MyEntityID", "MyEntityType");
```

# Events

Events can modify state and relationships of entities.

## Register Events

```csharp
var e = new Event()
 {
     On = "2001/01/01 01:01",
     Type = "MyEventType",
     Entity = "MyEntityID",
     Observations = new string[] { "State.MyVal=Hello World" }
 };

service.RegisterEvent(e);

```
