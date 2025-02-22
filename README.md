# Major Update as of Nov 23, 2018

The old labstreaminglayer repository has moved to https://github.com/sccn/lsl_archived . 

If you cloned/forked labstreaminglayer before the move, then you must redo this to get the latest source code. Please note that this repository uses submodules so you will have to do a recursive clone: `git clone --recursive https://github.com/sccn/labstreaminglayer.git`.

This repository is a container for all labstreaminglayer-related repositories.
The core library (liblsl) is hosted in the sccn organization.
Language wrappers and apps are hosted in the https://github.com/labstreaminglayer organization.

If you have any outstanding issues that need addressing then please comment on them in the lsl_archived repo. We will continue to monitor that repo and will transfer issues here as needed.
If you were working on any changes against the old repo then you can make a pull request against lsl_archived and we will take care of applying it to the new repo.

# Summary

The **lab streaming layer** (LSL) is a system for the unified collection of measurement time series
in research experiments that handles both the networking, time-synchronization, (near-) real-time
access as well as optionally the centralized collection, viewing and disk recording of the data.

The **LSL distribution** consists of:
- The core transport library
([liblsl](https://github.com/sccn/liblsl/)) and its language interfaces
  ([C](https://github.com/sccn/liblsl/),
  [C++](https://github.com/sccn/liblsl/),
  [Python](https://github.com/labstreaminglayer/liblsl-Python/),
  [Java](https://github.com/labstreaminglayer/liblsl-Java/),
  [C#](https://github.com/labstreaminglayer/liblsl-Csharp/),
  [MATLAB](https://github.com/labstreaminglayer/liblsl-Matlab/)).
  The library is general-purpose and cross-platform (Win/Linux/MacOS/
  [Android](https://github.com/labstreaminglayer/liblsl-Android/)/iOS, x86/amd64/arm)
  and forms the heart of the project.
- A suite of tools built on top of the library, including a
  [recording program](https://github.com/labstreaminglayer/App-LabRecorder),
  [file importers](https://github.com/sccn/xdf),
  and apps that make data from a range of
  [acquisition hardware](https://github.com/sccn/labstreaminglayer/wiki/SupportedDevices)
  available on the lab network (for example audio, EEG, or motion capture).

There is an intro lecture/demo on LSL here: [http://www.youtube.com/watch?v=Y1at7yrcFW0](http://www.youtube.com/watch?v=Y1at7yrcFW0)
(part of an online course on EEG-based brain-computer interfaces).

You may also wish to subscribe to the [LSL mailing list](https://mailman.ucsd.edu/mailman/listinfo/lsl-l)

This is only a repository containing submodules for all projects. Depending on your needs, you should
either download ready-to-use binary packages or clone just the repositories you need (see
[liblsl](https://github.com/labstreaminglayer/labstreaminglayer/tree/master/LSL) for an overview).

## Download Binary Releases

You can find *old* releases on our FTP site: ftp://sccn.ucsd.edu/pub/software/LSL/.

These releases are out of date. We are working toward an automated build and deployment system
but it is not ready yet.

## Streaming Layer API

The liblsl library provides the following **abstractions** for use by client programs:

- **Stream Outlets**: for making time series data streams available on the lab network.
  The data is pushed sample-by-sample or chunk-by-chunk into the outlet, and can consist of
  single- or multichannel data, regular or irregular sampling rate, with uniform value types
  (integers, floats, doubles, strings). Streams can have arbitrary XML meta-data (akin to a
  file header). By creating an outlet the stream is made visible to a collection of computers
  (defined by the network settings/layout) where one can subscribe to it by creating an inlet.

- **Resolve functions**: these allow to resolve streams that are present on the lab network
  according to content-based queries (for example, by name, content-type, or queries on the
  meta-data). The service discovery features do not depend on external services such as zeroconf
  and are meant to drastically simplify the data collection network setup.

- **Stream Inlets**: for receiving time series data from a connected outlet.
  Allows to retrieve samples from the provider (in-order, with reliable transmission,
  optional type conversion and optional failure recovery). Besides the samples, the meta-data
  can be obtained (as XML blob or alternatively through a small built-in DOM interface).

- **Built-in clock**: Allows to time-stamp the transmitted samples so that they can be mutually
  synchronized. See Time Synchronization.

## Reliability

The following reliability features are implemented by the library (transparently):
- Transport inherits the reliability of TCP, is message-oriented (partitioned into
  samples) and type safe.

- The library provides automatic failure recovery from application or computer crashes to minimize
  data loss (optional); this makes it possible to replace a computer in the middle of a recording
  without having to restart the data collection.

- Data is buffered both at the sender and receiver side (with configurable and arbitrarily large
  buffers) to tolerate intermittent network failures.

- Transmission is type safe, and supports type conversions as necessary.

## Time Synchronization

The lab streaming layer comes with a built-in synchronized time facility for all recorded data which
is designed to achieve sub-millisecond accuracy on a local network of computers.
This facility serves to provide out-of-the-box support for synchronized data collection but does not
preclude the use of user-supplied alternative timestamps, for example from commercial timing
middleware or high-quality clocks.

The built-in time synchronization is designed after the widely deployed Network Time Protocol (NTP)
and implemented in the LSL library.

This feature is explained in more detail in the
[TimeSynchronization](https://github.com/sccn/labstreaminglayer/wiki/TimeSynchronization.wiki) section.

## File Format

The transport API itself does not endorse or provide a particular file format, but the provided recording
program (`LabRecorder`) <!--and Python/C++ library (`RecorderLib`)--> records into the XDF file format
([Extensible Data Format](https://github.com/sccn/xdf)). XDF was designed concurrently with
the lab streaming layer and supports the full feature set of LSL (including multi-stream container
files, per-stream arbitrarily large XML headers, all sample formats as well as time-synchronization
information).

## Developer Information

Please see the separate [build documentation](doc/BUILD.md).

The distribution includes a range of code examples in C, C++, Python, MATLAB, Java, and C# including
some very simple sender and receiver programs, as well as some fairly extensive demo apps.

See [ExampleCode](https://github.com/labstreaminglayer/App-Examples/) for a broader
overview of example programs, API documentation link, and general programming tips, tricks, and
frequently asked questions.

## How to get support

If you are having trouble with LSL, there are few things you can do to get help.
First, look to the [Wiki](https://github.com/sccn/labstreaminglayer/wiki), especially the [frequently asked questions](https://github.com/sccn/labstreaminglayer/wiki/Frequently-Asked-Questions).
Second, search the issues list, including closed issues, in this repository and in the archived repository [here](https://github.com/sccn/lsl_archived).
If you don't find what you are looking for, then you can create a new issue. Try to include as much
information as possible about your device (if applicable), your computing environment (operating
system, processor achitecture), what you have tested so far, and also provide logs or other error
messages if available. If you create a new issue then please be responsive to follow-up questions and be sure to close it once the issue is solved.

You can also try joining the LabStreamingLayer `#users` channel on Slack. [Invite Link](https://join.slack.com/t/labstreaminglayer/shared_invite/enQtMzA2NjEwNDk0NjA5LWI2MmI4MjBhYjgyMmRmMzg2NzEzODc2M2NjNDIwODhmNzViZmRmMWQyNTBkYzkwNmUyMzZhOTU5ZGFiYzkzMzQ).
Someone there may be able to get to the bottom of your problem through conversation.

## Acknowledgements

The original version of this software was written at the
[Swartz Center for Computational Neuroscience](http://sccn.ucsd.edu/people/), UCSD.
This work was funded by the Army Research Laboratory under Cooperative Agreement Number
W911NF-10-2-0022 as well as through NINDS grant 3R01NS047293-06S1.
