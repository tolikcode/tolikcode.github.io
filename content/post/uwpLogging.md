+++
date = "2015-10-24T23:23:55+03:00"
draft = true
title = "Logging in Universal Windows applications"

+++

I was always jealous of Android where you get a (handy) logging out of box with Logcat. 

With Windows 10 being released recently I was hoping to have some handy out-of-the-box way to do logging. Alas. There are some changes to ETW Tracing and that's all. If your have a Windows Phone / Window 8 development experience  there would be nothing new for you.


## ETW Tracing
The Microsoft way for logging in UWP applications is via Event Tracing for Windows(link). You can find a corresponding sample from Microsoft on [GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Logging). This technology has been around both in desktop and mobile versions of Windows for quite some time already. 

<span style="color:green;font-weight:bold;font-size:large">+ </span> Out of the box. No need to reference any additional libraries.

<span style="color:green">+ </span> Semantic logging.  Your can analyze it with different tools

<span style="color:red">- </span> requires effort to configure

<span style="color:red">- </span> requires effort to decode *.etl files

## MetroLog

I'd like to have log4net experience.

There are many different 3d party libraries for logging in UWP
However the most popular one is MetroLog(link)

+ easy to configure
+ Plain old txt files. Metro logs can output to different targets, but it's really simple to configure it to log everything into a *.txt file.
- additional reference to your application

## Logs retrieval for Windows phone
To get logs from users you can upload them to some server or just send them to your server via email. In case you have a physical access to a test device 
the easiest and most convenient way is to use Windows Phone PowerTools(link). I've tested it with Windows 10 Mobile Developer Preview {{developer?}} devices and it works great.