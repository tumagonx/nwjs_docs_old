Validation, or review process, is the main concern when developing an app for the Mac App Store.

The validation steps are listed in the [Apple documentation](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/ChangingAppStatus.html).

## Before the validation

* Despite NW.js allowing you to do whatever you want with the UI, you have to follow the OS X guidelines (you can find help in existing tools to achieve this - like the [Maverix theme](https://github.com/tschundeee/maverix), for instance)
* Check the behavior of the app, and visual glitches
* Explain and describe. There are fields in iTunes Connect to help the App Review team, to describe the entitlements used, or to write comments; use them! If you have an online manual, send them the link.
* Use entitlements carefully, only select the ones that you really need
* Be sure that your app is suitable for the Store: [Common app rejections](https://developer.apple.com/app-store/review/rejections/)

## Average delays

You have to wait 4-7 days between your submission and the beginning on the review process ([Average App Store review times](http://appreviewtimes.com/)).

If you need to publish an urgent fix, you can request an [expedited review](https://developer.apple.com/contact/app-store/?topic=expedite). The delay will then drop to 1-3 days.
If you never used it and you have a good reason (like an app crash), it will be accepted.

When the validation process starts, you will be notified by email.

## If your app is rejected

The rejection reason will be explained in the Resolution Center (in iTunes Connect).

There are two major scenarios:

* The validation team pointed out a small bug. If it is easy to fix, do so. You can rebuild your app, submit the new build, and tell it's done on the Resolution Center. The validation will then continue within hours. Here are some cases:
  * You forgot to remove an entitlement
  * There is a typo
  * They found a visual glitch
  * They don't understand a feature (do not hesitate to explain how your app behaves when sending your answer, send screen captures, etc)
* There is a major issue that needs more work; you can remove your build and cancel the review process. But you will have to wait another week when you will resubmit your app.

Good luck!