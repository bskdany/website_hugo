---
title: "Abusing Bluetooth Speakers"
date: 2023-06-22T02:41:32-04:00     
---

One day in April 2022 I noticed how a speaker used by a restaurant close by had the pairing mode always on, I obviously found it fun to connect to it every time I was close by and play [Scotland Forever](https://www.youtube.com/watch?v=f2bHoTUiMpI) at max volume, making the waiters run around confused...

Anyways, that made me wonder, how many more of those speakers can be found? I already had a plan in mind, an android app that automatically scans, pairs, connects and plays loud music on whatever it can connect to. How can I make this a real thing? I have no idea, I never did an android app nor had to deal with Bluetooth.

I tried to find some already existing android apps that can connect to Bluetooth devices, I did found a couple but I couldn't figure out the old versions Gradle to compile them into an apk, so I gave up on this, I had to do one from scratch.  

I moved to tutorials, I found [this](https://www.youtube.com/watch?v=Oz4CBHrxMMs) great video that shows the basics on how to move around Android Studio and the logic behind android apps Bluetooth capabilities, [the official android API documentation](https://developer.android.com/guide/topics/connectivity/bluetooth) was also very useful. A problem that I encountered was the choice of the language: I didn't really want to explore Javas boilerplate so I opted to stick to Kotlin in the beginning, after messing around here and there I realized that the Bluetooth documentations wasn't up to date for Kotlin and so I had to switch back to Java.

After a long week of trial and error I finally managed to create a functional app

!["Screenshot of the app"](/app_screenshot.jpg)

I like how I had to install the app on my new phone at the time of writing this blog post to get this screenshot and it instantly crashed because apparently I was to lazy to request the "Nearby Devices" permission.

The app worked like this: it scanned all the Bluetooth devices around every 20 seconds and listed them on the screen, you could manually connect to a device by tapping one on the list or wait for the app to detect a connectable speaker and pair to it. The app had also to check if the device was already paired so that it could connect to it directly, I also implemented a "Stop Scan" button that would work as and "Abort Everything" button because things went sideways very often.

Two problems that made this app not as great as I imagined:
1. It wasn't doable to make this thing go in the background, at least I didn't figure out how, so I had to leave the app always open.
2. To pair a device automatically you would need the "BLUETOOTH_PRIVILEGED" permission that only system apps have, there is an actual workaround with adb where you can just push the apk to the system folder but it was giving me problems so I gave up.

As you may have noticed the part where the app automatically plays music is missing, this is because I used Tasker and AutoInput as a workaround to the automatic pairing and so I opted to create the music part with Tasker too because I was feeling too lazy to code it.

Those are the Tasker rules that I created.
- if Bluetooth speaker is connected -> play Scotland Forever from Spotify
- if you turn volume down -> volume goes up
- if you stop the song -> song starts playing again

It was time to test my creation, so I get my phone, set everything up and walk past the restaurant. Surprisingly it managed to do all of the pairing, the connecting and the playing of the music on the first try. The gentle tune of Scottish music started playing at max volume as I try not to make eye contact with the confused waiter standing next to me.

Using the app a lot somehow damaged the Bluetooth chip on my phone and I'm pretty sure that the restaurant figured out who I was, 10/10 would do again.

The source code and the compiled app can be found [on my github](https://github.com/bskdany/AndroidBluetoothAutoPair).