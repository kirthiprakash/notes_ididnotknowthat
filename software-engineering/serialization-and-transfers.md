# Serialization and transfers

## Notes

### [https://engineering.fb.com/android/improving-facebook-s-performance-on-android-with-flatbuffers/](https://engineering.fb.com/android/improving-facebook-s-performance-on-android-with-flatbuffers/)

* **Parsing speed.** It took 35 ms to parse a JSON stream of 20 KB \(a typical response size for Facebook\), which exceeds the UI frame refresh interval of 16.6 ms. With this approach, we were unable to load stories on demand from our disk cache without observing frame drops \(visual stutters\) while scrolling.
* **Parser initialization.** A JSON parser needs to build a field mappings before it can start parsing, which can take 100 ms to 200 ms, substantially slowing down the application start time.
* **Garbage collection.** A lot of small objects are created during JSON parsing, and in our explorations, around 100 KB of transient memory was allocated when parsing a JSON stream of 20 KB, which placed significant pressure on Java’s garbage collector.

