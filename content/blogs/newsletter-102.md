---
title:       "Newsletter 102 - 2022/2023 news"
author:      "GeoB99"
date:        2023-03-25
tags:        [ newsletter ]
---

Hello ReactOS followers and enthusiasts! It has been quite a long time since we published the 101th newsletter and so far no further updates have been posted since then.
While the [ReactOS Twitter account](https://twitter.com/reactos) does provide announcements, posts about working applications and such from time to time, much of what is happening with the project
as a whole isn't mentioned. Most of what we are going to talk about is the current situation with releases and overall ReactOS development.

## Why does it take so long to release a new version of ReactOS? Are you guys dead?
The latest release of ReactOS is 0.4.14, published on 16th of December, 2021. That release alone took a year to be released. Back then the ReactOS followed the 3-months cadence, which every 3 months
a new release would be released to the public. But since 2021, ReactOS is still at 0.4.14. Are you guys dead?

The answer is certainly no, the way we handle releases has changed. Back in the day the way ReactOS releases were made it was done for the sake of quantity than quality. Every new release after 3 months
is similar to each other, with exceptions of a very few features and bug fixes shipped to the release. But overall the net gap between the releases wasn't that big.
Now there is a rule that for a development release to reach "Release Candidate" status, it has to have a reasonable amount of regressions (no more than 20) and that the stability isn't badly impacted
by the introduction of new features or code changes done during the development. In other words, the ReactOS project is now focusing on providing quality releases of which the 3-months cadence cannot
be applied anymore. Such approach wasn't quite feasible for a development team size like ours anyway.

So it is expected that it will take longer for a new release to be published, although such releases will now be substantial with lots of improvements than they were before.
If you're impatient to test the latest changes and features you can always try the [nightly builds](https://reactos.org/getbuilds) (also called experimental builds).

## Development status
In the last year and during the beginning of 2023, the ReactOS developers and contributors alike are working on many fronts of the project, the top focused area being the kernel.
Other areas that isn't kernel related are the applications, specifically our Paint and Notepad programs, the Input Method Editor (IME) as well as other stuff such as the ReactOS
testcase infrastructure.

## ReactOS x64 port status
The x64 port of ReactOS, while still in its infancy, has steadily gotten better in terms of stability. Back then ReactOS could barely boot and if it ever did, it usually resulted
in an immediate crash or a BSoD by just doing simple tasks. With the help of Timo Kreuzer (tkreuzer), the x64 port has gotten to a somewhat tolerable state where ReactOS can now
operate without big problems. Although issues still do remain, for instance audio does not currently work in x64, more apps have started to work in one way or another.

=== ADD SOME SCREENSHOTS HERE ===

## WHS test fixes
WHS testcase infrastructure was known to fail a lot of times due to tests not passing in WHS. On January of 2023 during the hackfest, Timo Kreuzer (tkreuzer), Mark Jansen (learn_more) and
Thomas Faber (ThFabba) worked together to fix some of the testcases that prevented the WHS test bot to pass all the tests. Thanks for their efforts, WHS can now pass all the tests.

## Paint & Notepad improvements
Thanks to Katayama Hirofumi MZ both Paint and Notepad have received quality of life improvements, miscellaneous fixes and whatnot. Notably, he fixed some flickering issues on Paint when
resizing the application's window and implemented the "text tool" feature. In addition, he improved the performance of Notepad, implemented the "Now Printing" dialog and provided some
miscellaneous fixes.

## Kernel Debugger overhaul
TO BE ADDED (hbelusca).

## Security Subsystem improvements
George Bișoc (GeoB99) has progressively improved the Security Subsystem (`Se`) of the kernel with new stuff as well as bug fixes, mostly notably improving the security access checks
and access tokens.

An access check is an action done by the kernel to check that if an object can be accessed by a requestor. If the requestor does not have the right privileges to access
an object or the object itself prevents others from doing certain actions imposed by an access control list (ACL), access is denied. There was a hack in `SepAccessCheck` where access
was granted to anybody, regardless of the security restrictions imposed by security descriptors. This could lead to bad actors tamper and toy around with objects.

Thanks to his work, the hack in `SepAccessCheck` has been removed and security access checks are now operational and the kernel can prevent any unauthorized access from whoever tries
to access an object with improper access rights. His future plan for the Security Subsystem is to implement security auditing of all objects and miscellaneous improvements where needed.

## Registry improvements
For ReactOS to reach Beta phase it has to solve two major milestones -- stability and corruptions. Usually corruptions in ReactOS can manifest in form of filesystem and registry corruptions.
While filesystem corruptions can be prevented by choosing a more modern filesystem such as BtrFS which is what we currently support, registry corruptions are by themselves a death
sentence as ReactOS currently doesn't implement any self-healing mechanisms to recover the system from corrupted registry hives. As a result, the user is forced to do a clean reinstallation of the system.
In worst cases, the system would still boot up but stuck in the login prompt dialog (aka the "Ctrl+Alt+Del" dialog) as the user profile has been corrupted or missing. The latter generally happens when one force shutdowns ReactOS after first clean install.

George Bișoc is currently working on improving the registry by implementing the necessary registry self-healing mechanisms and `CmCheckRegistry`, a fundamental function that validates the registry
for any corrupted parts of the registry and takes appropriate actions at recovering a registry hive from such corrupt parts. While his work is currently in progress ([#4571](https://github.com/reactos/reactos/pull/4571)), so far he has received some positive results from testers. A small piece of his work has been merged, which fixes the registry flusher of ReactOS not working for almost a decade! This fix also
solves the problem of the missing user profile due to force shutdown of the system, as such you should no longer expect the infamous "Ctrl+Alt+Del" dialog prompt.

In addition he is working on the registry cache implementation, as ReactOS doesn't properly sync the cached information thus leading to inconsistent results.

## IME project
The Input Method Editor, sometimes abbreviated as IME, is a component that facilitates the typing of characters not originally present on the input devices by using a sequence of characters.
IME in ReactOS has been for the most part a stub, but thankfully Katayama Hirofumi MZ has extensively worked on IME.
This greatly helps with CJK support as well as allowing installation of custom IMEs for different locales.

## Beyond the main tree
ReactOS has always been more than just an operating system.
We are a community of people focused around the Windows ecosystem — with various projects to research and document Windows internals, improve Windows support in applications, or otherwise make life easier for the broader Windows developer community.
In fact, most developers likely had their first encounter with ReactOS not by downloading the operating system, but when looking up an undocumented API on the web and ending up in our [Doxygen-generated documentation](https://doxygen.reactos.org).

Consequently, much work within the past two years also happened outside our main source tree, and it's worthwhile to take a look at that:

Colin Finck has been trying out the Rust programming language for Windows development, and he has released multiple related libraries.  
The first one, [nt-hive](https://github.com/ColinFinck/nt-hive), initially released in February 2021, implements a parser for the internal structures of Windows registry _hive_ files.  
It was followed by [ntfs](https://github.com/ColinFinck/ntfs) in January 2022, a more ambitious project to provide an NTFS filesystem reader as a reusable building block for various scenarios.
That library has been released at the virtual FOSDEM 2022 along with a [presentation on NTFS internals](https://archive.fosdem.org/2022/schedule/event/misc_ntfs_rust/).  
The latest addition is [nt-list](https://github.com/ColinFinck/nt-list) later that year, which is a type-safe and idiomatic Rust wrapper around Windows Linked Lists (otherwise known as `LIST_ENTRY` and `SINGLE_LIST_ENTRY`).
It has been [presented at EuroRust 2022](https://www.youtube.com/watch?v=IxhZIyXOIw8).  
A library for parsing apiset DLLs is currently in the pipeline.

Ultimately, these libraries shall all be used in a Rust-powered bootloader.
But each of them can already be used now in various contexts, from low-level system development to user-mode applications.
And this is already happening.
