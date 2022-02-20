---
layout: page
title: wxiOS, the wxMobile API implementation in Cocoa Touch
---

(Accepted and successfully completed project, see [demo](https://www.youtube.com/watch?v=PTbFj9dqROs); [original proposal](https://www.wxwidgets.org/gsoc/2011/).)

The wxiOS project is an effort to create a wxWidgets based C++ wrapper over Cocoa Touch that would implement the proposed wxMobile API. The implementation would allow creating mobile UIs in a more generic, platform-independent way. If, for example, an analogue gets implemented for Android OS, one could expect to run the same end-user mobile application code on both mobile platforms without too much hassle.


## Proposal

### Abstract

As of today, there is no feasible way to create a native (compiled) application for several mobile platforms at once. One way to narrow the gap between various target platforms (iOS, Android, …) would be to implement [wxMobile API](http://www.anthemion.co.uk/mobile/docs/mobile/manual/html/index.html) proposal for the various UI toolkits these platforms use.

The wxiOS project is an effort to create a wxWidgets based C++ wrapper over Cocoa Touch that would implement the proposed wxMobile API. The implementation would allow creating mobile UIs in a more generic, platform-independent way. If, for example, an analogue gets implemented for Android OS, one could expect to run the same end-user mobile application code on both mobile platforms without too much hassle.

### Benefits

1. A first actual (not simulated) wxMobile implementation would allow developers to create applications that would be more easily portable to other mobile platforms in the future. The portability would, of course, require that the API would be implemented in other platforms too, e.g. on Google Android (using [Android NDK](https://developer.android.com/ndk) or [android-cpp-sdk](https://code.google.com/archive/p/android-cpp-sdk/)), Windows Mobile 6 and others.
2. Also, an application developer would not be required to use the vendor-specific application development platform and toolchain (in this particular case, Mac OS X with iPhone SDK) until the later stages of the application development. It is expected that the developer could use the current implementation of wxMobile simulator to develop the application and would only be required to use the vendor’s platform for the final testing stage.

### Project goals

* Implement the current wxMobile API (`wxMo*` classes as outlined in the current class list) using Cocoa Touch:
    1. Adapt code from `src/osx/iphone/` where the implementation for the functionality in question already exists
    2. Implement the remaining classes
* Extend the wxMobile API proposal in such ways:
    1. Create new classes that cover the basic UI elements not currently in the wxMobile proposal, e.g. `UIDatePicker` → `wxDatePicker`
    2. Extend the current proposal classes to contain more properties, e.g. extend `wxMoTextCtrl` to work for password text entries, or create a separate class for such text entries (`wxMoPasswordCtrl`)
    3. Create new classes mimicking the wxWidgets standard controls that would be mapped not to a single UI control (such as `wxActionSheet` → `UIActionSheet`) but to a group of UI controls, e.g. `wxComboBox` could be implemented as a table view (`UITableView`) with choice options and an ability to enter a custom option
*  Create an example application (*wxMobileCatalog*) that would showcase the wxMobile implementation in Cocoa Touch (think of *UICatalog*).
    1. The application will be a close clone of the *UICatalog*, reimplementing all the examples with wxMobile API
    2. A simple, comprehensible `README` should follow the application explaining how to build and run it

### Success criteria (deliverables)

1. At least all of the current wxMobile API proposal classes are implemented using Cocoa Touch
2. A working and stable example application *wxMobileCatalog*, a "wx" clone of *UICatalog*, is provided

### Schedule

I would not be able to start coding full time until June 3 because of the course work at the university.

After that date, I could work on the project at least for 40 hours/week (as expected).

1. **April 26 - June 2** (38 days):
    * Getting to know the wxWidgets developer community.
    * Discussing details of how and when to report my progress to the mentor.
    * Discussing ways to proceed with a student implementing the wxAndroid project (if any).
    * Familiarizing myself with the current wxWidgets library architecture.
    * Analyzing which parts of the Cocoa Touch mapping to wxWidgets are already implemented in the trunk.
    * Following the `wx-dev` mailing list, lurking on the `#wxwidgets` IRC channel.
2. **June 3 - June 30** (28 days):
    * working on Milestones 1-2.
    * constant progress reports and discussions with the mentor and a student implementing the wxAndroid project (if any).
3. **July 1 - July 15** (16 days):
    * working on Milestone 2.
    * constant progress reports and discussions with the mentor and a student implementing the wxAndroid project (if any).
    * preparing the code for the mid-term evaluation (bug fixes, unit tests).
4. **July 16 - July 31** (16 days):
    * working on Milestone 2.
    * constant progress reports and discussions with the mentor and a student implementing the wxAndroid project (if any).
5. **August 11 - August 20** (10 days):
    * working on Milestone 3.
    * cooperation with wxAndroid project to unify the way of building the applications and the style of documentation.
    * preparing the code for the final evaluation (bug fixes, unit tests, documentation, "final touches"). 

### Milestones

* by **June 30**: implementation of at least the basic controls that require one-to-one mapping, e.g. `wxMoActionSheet` (wrapper for `UIActionSheet`), `wxMoTextCtrl` (wrapper for `UITextField`, etc.), plus a barebone *wxMobileCatalog* application
* by **July 31**: implementation of at least all the classes from the current wxMobile proposal, plus an almost finished *wxMobileCatalog* application
* by **August 20**: unit tests, flexible building scripts, improved code comments and documentation


## Me

### Why wxiOS

It would be great if there finally was a way to create a single native application for both the iOS and Android platforms (I really hope someone steps up for the [wxAndroid project](http://wiki.wxwidgets.org/Development:_Student_Projects#wxAndroid_port) too). HTML5+JavaScript based solutions just don’t work that well right now. I would definitely be one of the first users of such a library myself.

### Why me

* I feel really enthusiastic of what could come out of this project,
* I learn fast,
* I don't trust my code so I test it a lot,
* I see the importance of developing a product using a single coding style,
* and I hope my English isn't too bad.

### wxWidgets / platform experience

* **Experience with C++:** a fair amount
* **Experience with wxWidgets:** a little
* **Experience with programming on Linux:** a fair amount
* **Experience with programming on Windows:** none
* **Experience with programming on Mac OS X:** a fair amount

### Experience

#### Relevant to the proposal

(Co-)created and published four iOS/Mac OS X applications so far: 

1. **[15min](http://itunes.apple.com/lt/app/id322573191?mt=8), the news reader for a local news outlet.** Features news articles, article comments, ability to post comments, weather forecast, photo gallery, daily voting poll, citizen self-reporting tool. Based on XML-RPC-like communication with the web server. Strong MVC separation, most of the code is backed by UI+logic unit tests, mature 3rd party libraries used.
2. **[Anglonas iPhone](http://itunes.apple.com/us/app/anglonas-english-lithuanian/id381153256?mt=8), the (only) English-Lithuanian dictionary for iOS.** Features a simple, minimalist UI; word definition bookmarks; some improvements to suit the iPad screen size too. Uses custom build of OpenSSL; Google Toolkit for Mac (GTM), FMDB library. Strong MVC separation so that the same code could be used in the Mac OS X version of the dictionary; backed by UI + logic unit tests.
3. **[Anglonas Mac OS X](http://itunes.apple.com/lt/app/anglonas/id411857989?mt=12), as in "we did this for the iPhone, now do this for the Mac".** Features simple yet "smart" UI (fixes common typos, switches keyboard layout when appropriate, comfortable shortcuts, etc.); integration into the Mac OS X via Services; superfast and not unnecessary-feature-bloated. Shares code with the iOS dictionary; OEM (non-Mac App Store) build has automatic updates, crash reports, antipiracy measures, downwards-compatible with 10.4-10.5.
4. [OpenDict + Mac OS X](opendict-macos), an OS X port of OpenDict, the open source dictionary (written in Python + wxPython).

#### Other

Been doing this and that to have fun and pay the rent:

* Public transport schedule websites for various cities (Vilnius, Lithuania, Riga, Latvia, Tallinn, Estonia, others) featuring a schedule timetable data and a trip planner.
* Stops Vilnius (also http://www.stops.mobi/vilnius/), a J2ME public transport schedule viewer for Vilnius (Lithuania) that works offline (contains the schedule data itself). Challenged myself of how much a ~200 MB schedule database could be squeezed, got down to ~700 KB (~100 KB when zipped), "invented" a indexed binary file format for storing the data :-)
* Three20 Cookbook, a wiki for a rather under-documented Three20 library. Official way of submitting documentation is still a bit sluggish (the official process being "write this in Markdown and make a git pull request"), so I thought having a wiki for myself (and others) would be better.
* Ported several applications to FreeBSD port collection: games/quake3, deskutils/kchm, net/tspc2 (now obsolete)

**Full skills boilerplate** (skills acquired in various commercial/hobby projects to varying degrees): PHP 4.x, PHP 5.x; C, C++, Objective-C (iPhone/Mac OS X SDK); Java (Java Mobile); HTML, XHTML, WML, CSS 1/2; JavaScript; DOM 1, DOM 2; XML, XSL(T); MySQL (since 3.23); Apache (1.3, 2.0, 2.2), lighttpd; Linux (Debian GNU/Linux, OpenSUSE, Ubuntu, Fedora, CentOS, …); FreeBSD (since 4.6-RELEASE); Mac OS X (since 1.4.10).
