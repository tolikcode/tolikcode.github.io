+++
date = "2015-10-24T23:23:55+03:00"
title = "Logging in Universal Windows applications"
+++

As a Windows Phone developer I always envied Android developers with their quite handy out of the box logging solution, namely LogCat. It allows both to output debug information in real time and to write it somewhere permanently.

With Windows 10 being released I was hoping to have something similar to Android LogCat in Windows. Alas, there are a few changes to ETW Tracing and that's all. If your have a Windows Phone / Window 8 development experience there is going to be nothing new for you.

## ETW Tracing
The Microsoft way for logging in UWP applications is via [Event Tracing for Windows](https://msdn.microsoft.com/en-us/library/windows/desktop/aa363668). You can find a corresponding sample from Microsoft on [GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Logging). This technology has been around both in desktop and mobile versions of Windows for quite some time already. 

<span class="largeGreen">+ </span> Out of the box. No need to reference any additional libraries.

<span class="largeGreen">+ </span> Semantic logging. Your logs are going to have a consistent structure that makes it easier to analyze them.

<span class="largeRed">- </span> Requires an effort to configure.

<span class="largeRed">- </span> By default the result is going to be `*.etl` files that have to be decoded.

## MetroLog

There are many different third party libraries for logging in Universal applications. The most popular one is [MetroLog](https://www.nuget.org/packages/MetroLog/). Log4net is a default logging solution in desktop applications for me. MetroLog is a log4net for Windows Universal applications.

<span class="largeGreen">+ </span> Easy to configure.

<span class="largeGreen">+ </span> Plain old text files. MetroLog can output to different targets, but it's really easy to configure it to write everything into a `*.txt` file.

<span class="largeRed">- </span> Additional reference in your application.

## Retrieving logs from a Windows phone
Whether you're doing ETW tracing or using a third party library (or your own custom solution) you are going to need to retrieve your logs from a device. You can upload them to some server or just prompt users to email them directly to you. In case you have a physical access to a test device 
the easiest and most convenient way is to use [Windows Phone Power Tools](https://wptools.codeplex.com). I've tested it with Windows 10 Mobile Insider Preview and it works great.

## Application Insights
In Visual Studio 2015 Microsoft has added [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) into Universal App project template. Application Insights is a comprehensive application monitoring solution available in Azure cloud. It allows to monitor the availability, performance, and usage metrics of different applications and services, and it might help you to solve the problems you wanted to solve with logging.