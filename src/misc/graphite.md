# Graphite

3 Software components

- **carbon**: service that listens for time-series data
- **whisper**: DB lib for storing time-series data
- **graphite-web**: Djangouser interface and API for graphs and dashboards


Whisper is a fixed-size database, similar in design and purpose to RRD (round-robin-database). It provides fast, reliable storage of numeric data over time. Whisper allows for higher resolution (seconds per point) of recent data to degrade into lower resolutions for long-term retention of historical data.

Carbon is responsible for receiving metrics over the network, caching them in memory for "hot queries" from the Graphite-Web application, and persisting them to disk using the Whisper time-series library.

 distinct metric sent to Graphite is stored in its own database file, similar to how many tools (drraw, Cacti, Centreon, etc) built on top of RRD work

 High volume (a few thousand distinct metrics updating every minute) pretty much requires a good RAID array and/or SSDs. Graphite’s backend caches incoming data if the disks cannot keep up with the large number of small write operations that occur (each data point is only a few bytes, but most standard disks cannot do more than a few thousand I/O operations per second, even if they are tiny). When this occurs, Graphite’s database engine, whisper, allows carbon to write multiple data points at once, thus increasing overall throughput only at the cost of keeping excess data cached in memory until it can be written.

 Graphite does not use RRD because 2 reasons:

 1. RRD assumes regular load
 2. RRD only allowed to insert 1 value at a time

