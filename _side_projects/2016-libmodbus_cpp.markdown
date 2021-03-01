---
layout: post
year: 2016
title: "libmodbus_cpp"
---

[Modbus](https://en.wikipedia.org/wiki/Modbus) is a data communications protocol for computer systems, sensors, and hardware actors. It is used for production pipeline automation and SCADA monitoring.

[libmodbus](https://github.com/stephane/libmodbus) is a popular library for Modbus, but it is written in pure C. `libmodbus_cpp` is a C++/Qt wrapper for easier integration into Qt projects that we were working on at that moment. Since the original library is an LGPL project, we decided to share the wrapper at GitHub.

Technologies used: C++, Qt.
 
Source code: [GitHub](https://github.com/binary-machinery/libmodbus_cpp)