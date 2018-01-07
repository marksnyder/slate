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

A powerful library written in .NET Core for searching and linking event/time driven datasets. Provides a rolling state mechanism and intelligent state linking.


## Source Code:

<a class="github-button" href="https://github.com/marksnyder/Chronicity/subscription" data-icon="octicon-eye" data-size="large" data-show-count="true" aria-label="Watch marksnyder/Chronicity on GitHub">Watch</a>

<a class="github-button" href="https://github.com/marksnyder/Chronicity/archive/master.zip" data-icon="octicon-cloud-download" data-size="large" aria-label="Download marksnyder/Chronicity on GitHub">Download</a>

<a class="github-button" href="https://github.com/marksnyder/Chronicity" data-icon="octicon-star" data-size="large" data-show-count="true" aria-label="Star marksnyder/Chronicity on GitHub">Star</a>

## Nuget Packages:

<a href='https://www.nuget.org/packages/Chronicity.Core/'>Chronicity Core</a>  

<a href='https://www.nuget.org/packages/Chronicity.Provider.InMemory/'>Chronicity.Provider.InMemory</a>

## Example:

<a href='http://ex.chronicity.io'>Bitcoin Timeline Example</a>

# Initializing

In order to use the timeline service you must create an instance of ITimeLineService.


> At this time an in memory provider is available. Future development will include persistent storage providers.

```csharp
var service = new Chronicity.Provider.InMemory.TimeLineService();
```



# Events

Events can modify the state and relationships of registered entities.

## Register New Event

```csharp
var e = new Event()
 {
     On = "2001/01/01 01:01",
     Type = "MyEventType",
     Entity = "MyEntityID"
 };

service.RegisterEvent(e);

```

## Setting Entity State

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

## Linking Entities

```csharp
var e = new Event()
 {
     On = "2001/01/01 01:01",
     Type = "MyEventType",
     Entity = "MyEntityID",
     Observations = new string[] { "Entity.Links.Add=AnotherEntityId" }
 };

service.RegisterEvent(e);

```

## Filtering Events

> Event Filter

```csharp
service.FilterEvents(new string[] { "Filter1", "Filter2" });
```


Registered events can be searched using expression filters. Filters will return a results that contains:

Property  | Description
--------- | ---------
State | The state of the entity after event was registered
Event | The matching registered event
Links | Entities linked after event was registered
LinkedState | State of linked entities after event was registered

Filtering options include:

Example | Description
------ | --------
Type=MyEventType | Event type
On.Before=2001/01/01 01:02 | Event time
On.After=2001/01/01 01:02 |  Event time
On.Between=2001/01/01 01:00,2001/01/01 01:02 |  Event time
Entity.State.MyVal=Hello World | Entity state

# Entities

Entities represent tracked state over time.


## Retrieving Entity state

```csharp

service.GetEntityState("MyEntityID","2001/01/01 01:01");

```

Returns a dictionary representing state at the time specified.


## Retrieving Entity links

```csharp

service.GetEntityLinks("MyEntityID","2001/01/01 01:01");

```

Returns a list representing links to other entities at the time specified.
