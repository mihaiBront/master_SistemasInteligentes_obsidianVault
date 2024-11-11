---
Subject: Big Data Analytics
Teacher: "@RafaelBerlangaLlavori @RafaelBerlangaLlavori"
Topic: Spark
date: 2024-11-11
tags:
  - "#SJK006-BigDataAnalytics"
---
# 9. What is a stream?
A data stream is an infinite sequence of events or observations:
- Data is usually associated with a timestamp 
- The observation/event is represented either as raw data (text), tuples (SQL) or as complex objects (JSON) 
- Data stream generators: 
	- Sensors (Internet Of Things -IoT-) 
	- Social networks (Twitter, LinkedIn, Facebook, etc.) 
	- News channels (Feeds) 
	- Any temporary measure (prices, indices, etc.)

They are the techniques to efficiently process data incrementally: 
- Real-time processing: 
	- **buffer** the streamed data 
	- **process** buffered data (the result is also data stream) 
	- **get rid** of buffered data (be careful not to discard data that could be interesting in the future)
- It requires **event-based programming** (e.g., Nodejs)
- It requires **window** operations (time-based buffers)
	- Tumbling and sliding windows 
	- Distribution & join of windows 
- Real-time vs batch processing

[PubNub](https://www.pubnub.com/demos/real-time-data-streaming/?show=demo)

# 9.2 Data streams in data science

Applications range from:
- Monitoring and prevention:
	- Alarms (detecting outliers in sensor monitoring)
	- Anti-fraud (detecting outliers in data from transactions)
	- Cyber-attacks (same)
- Agents and decision making
	- Buy/Sell
	- Marketing

[Concept drift](https://en.wikipedia.org/wiki/Concept_drift) is a concept somewhat related to the fitting of a model; the necessities of which can change in time, rendering a model obsolete. When that happens, it needs a retraining.

**Casual associations** can happen between data streams and external complex events:
- Breakdowns -> Stocks
- Accidents -> Reputation
- ...
## Event hubs

![[Pasted image 20241111182702.png]]

Generally speaking, in a real application, there will be more than one event source, that's why *hubs* exist, to concentrate events into a single service.

Another thing to account for is that the events don't necessarily come in the right order -> **Fault Tolerance**
## Combining streams

![[Pasted image 20241111183142.png]]
Sometimes, to ease data processing, events are first sent to processors ($Q_n$) which are in charge of combining events to provide a preprocessed, easier to interpret by the final data processor, data.
