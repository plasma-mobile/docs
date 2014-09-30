planning
========

Stuff to do
this is me starbuck, robin, pier
Prerequisites:

A device matching the HW platform (not immediately necessary, some stuff can be done independent of it, i.e. on x86)

Carsten says Nexus 4 adaptation is considered "solved" (yep, this is what I was trying to outline in my mail)

Moto G is comparable to Nexus 4 (thus not the exact same): getting a Nexus 4 device seems safer

No chances to buy a new Nexus 4 though

I wonder if maybe we should look at N5 instead? they're still available, and also quite well supported (http://wiki.merproject.org/wiki/Adaptations/libhybris)

Fine by me as long as they are supported :)

*Step 0 - Bootable text-based system (pier?)

Definitions: at this stage, we have something we can flash to target HW, and can boot, but can't really do anything useful with

Time to reach: ETA 8 Oct~

I say we don't bother doing a build from scratch but continue the way Maui is working now (new/updated packages on top of Mer binaries), but we'll need to get there at some point. Maybe after we can make phone calls?

sounds OK. make sure to check git/etc for updated packages. Jolla has done quite a lot of them here and there, but Mer core is currently not in an easy to work with state. Carsten is working to address that, but it's slow going.

git where, git.merproject.org? btw home:plfiorini:maui:devel builds on top mer-core:devel

this is part of the "not easy to work with" thing, that's one place, there's also e.g. https://github.com/mer-packages/, mer-hybris, mer-qt (though probably not needed if you're using 5.3), etc

*Step 1 - HADK-based setup (pier/robin?)

Definitions: at this stage, we have something we can run basic graphical tests on (e.g. minimer, qmlscene, whatever) on top of the framebuffer, no Wayland necessarily at the start)

Time to reach: ETA October ~15?

<<< we expect to package plasma somewhere around this timeframe >>>

-> pier/robin?

*Step 2 - Simple compositor (pier/robin?)

Definitions: at this stage, we boot to a simple compositor which can display Wayland clients

Time to reach: ETA October ~22?

The compositor, at this stage, will simply display at least one single fullscreen client

We expect that Plasma will display as a fullscreen thing, and "own" the homescreen, but this may not happen in time for step 2's completion

*Step 3 - KDE Frameworks (pier/robin?)

Check if a Plasma session is already capable of running on QML compositor

startkde will need some tweaks

qtsvg pulls in qtwidgets, plasma uses svg for the ui -> ends up with much fatter resource requirements

maui uses qml to style its ui, since the plasma guys are finally working on using qtquick controls (although styled with svg) i want to make plasma load custom styles like i do with the hawaii components but i'm not sure i can prepare a patch for this in time as it was supposed to be post maui 1.0

maybe eike can help bring the patch come to life, later we can build packages without qtwidgets for arm and the phone shell will be using qml styles; anyway for a prototype it might be good as it is now

*Step 4 - Telephony MW (pier/robin?)

Definitions: at this stage, we integrate the available pieces to get telephony working, if we didn't have them already

Time to reach: few days max (just packaging work)

We need to package (all available in Nemo, and we *should* use these versions, as they are known to work together):

ofono
ofono-ril
ofono-qt (ofono -> Qt bindings)
voicecall-manager (provides ofono -> QML bindings)

*Step 5 - Telephony UI (eike?)

Definitions: at this stage, we end up with a basic dialer that can send (and maybe recieve) phonecalls

Time to reach: for sending, few days max (sending calls is going to be significantly easier than recieving them, as recieving them requires compositor work for dialog support etc)

Build a dialer UI :-)

UI can be prototyped even on PC with Xorg

just need to keep expectations realistic, i.e, at this stage I think "a dialpad, text entry, and make call / end call" button would be about all we'd want

<<after prototyping phase/phone calls>>
Step ? - Mobile Wayland protocol for applications - let's give some more thought *after* we can make phone calls

Needed to properly display/interact Qt applications on mobile

We only consider applications written with Qt 5

Shouldn't be more complicated than display maximized windows

Don't deal with modal dialogs and popups for now

Needs:

qtwayland client protocol

Compositor side

*Open questions:

How's Plasma's Wayland port looking? (current status: preparing to get more manpower spread across tasks, martin g. responsible, see: https://todo.kde.org/?controller=board&action=show&project_id=2  - needs kde.org ident for login)

Things I did:

ECM macros for wayland-scanner -> pending review

drafted wayland protocols (.xml) -> almost good at this point but things may be changed during the development if we face something we didn't consider during the analysis back on august

implemented plasma-shell and plasma-effects protocols for kwayland (a library shared by components that need to use the wayland protocols)

martin g. wants to use his ConnectionThread but QPA already has a connection so if a client uses ConnectionThread the wl_surface taken from QWindow using QPlatformNativeInterface is not recognized by that connection;

see https://git.reviewboard.kde.org/r/120329/  maybe robin has some idea

working on plasmashell, ksplashqml and plasma-framework to bring wayland support (patch WIP in local repo)

ksplashqml already works on wayland in my local branch on green island (qml based compositor)

plasmashell partially works (can display desktop and panels, plasmoid window position is off because plasma-framework still need some work) on green island (qml based compositor)

no review of the work done so far on plasma-workspace was started, mostly blocked by the kwayland changes not being finished/approved yet

more man power on this would be better because looks like part of my time will be devoted to the OS (sebas? notmart?)
