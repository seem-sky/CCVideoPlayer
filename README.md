VideoPlayer
==================

Simple Video Player for Cocos2D apps.
This repo contains iOS & Mac XCode projects, demonstrating usage of VideoPlayer.


Features
-------------

   * Universal support (iPhone + iPad + Mac)
   * Play / Cancel (by tap or key pressed on mac)
   * Easy to use


Limitations
---------------

1. It's recommended to call VideoPlayer methods from main thread. On Mac OS X 10.6.5+ it hangs up if play started from thread other than main. (Probably it's easy to integrate performSelectorOnMainThread right into VideoPlayer. Issue #2 )


Usage
-----------------------

VideoPlayer by itself is located in Classes/VideoPlayer folder.
CustomVideoView.nib is located in Resources and needed only for Mac. (Probably it's possible to remove it and load view with code only. Issue #3 )

Also it needs a little bit changed MacGLView - it was modified in commit with SHA: 3b5339070dadab02e9b7381c2becebbd616ade31

Without modified MacGLView VideoPlayer will compile with warnings and crash at starting playback on a Mac ( Probably it's possible to fix this, so it will work without modifying MacGLView, but cancelling video by pressing a key will not work. Issue #4 )

To link it you need MediaPlayer.framework for iOS & QTKit for Mac

To play videofile foo.mp4 simply use:

    [VideoPlayer playMovieWithFile: @"bait.mp4"];

VideoPlayer ignores orientation change by itself, but you can manually change it's orientation:

    UIDeviceOrientation deviceOrientation = (UIDeviceOrientation)toInterfaceOrientation;
    [VideoPlayer updateOrientationWithOrientation: deviceOrientation ];

Playing video uses a lot of resources, so it's recommended to stop gpu render and other heavy tasks, while playing video.
You can do this by setting a VideoPlayer delegate:

    [VideoPlayer setDelegate: self]; 

Your delegate class should conform to VideoPlayerDelegate and implement these methods:

    - (void) moviePlaybackFinished
    {
        [[CCDirector sharedDirector] startAnimation];
    }

    - (void) movieStartsPlaying
    {
        [[CCDirector sharedDirector] stopAnimation];
    }

It's a weak link, so don't forget to set delegate to nil in dealloc.


Supported Video Formats
----------------------------

Supported formats are the same as for MPMediaPlayer for iOS and QTMovie for Mac.
mp4 bundled with tests is compatible with both Mac & iOS


Contribution
-----------------------------
VideoPlayer is a part of stuff, that i used in [iTraceur - Freerunning / Platform Game][iTraceurLink].
I would like to share with Open Source community as much as possible, cause Open Source helped me a lot with iTraceur.
So questions, suggestions and corrections are always welcome.

[iTraceurLink]: http://itunes.apple.com/us/app/itraceur-parkour-freerunning/id374163905?mt=8& "iTraceur App Store Link"

