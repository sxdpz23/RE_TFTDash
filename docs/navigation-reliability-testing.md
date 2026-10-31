# Navigation reliability test - 0.2.0-dev01

Use a parked bike for setup, connection and controls. Record phone model, Android build, console firmware and motorcycle/year with every result. No software is installed on the console.

1. Install the development APK; allow precise location, nearby Wi-Fi and notifications. Start the connection with the app visible. Follow the existing battery optimisation guidance.
2. Confirm accurate position in open sky, the foreground notification and map streaming. Set a destination using existing single-destination navigation. Multi-stop routing is not in this build.
3. Lock with the power button for 10 minutes. Unlock and verify movement resumed without a stale-position jump. Repeat with display timeout and switching to another app.
4. With a passenger/test observer or a safe stationary replay arrangement, compare phone position age and console display latency. Never interact with the phone while riding. Complete a 60-minute route, then a two-hour soak; include stops, bends and a genuine missed turn.
5. Test a GPS outage and restoration. The map must indicate GPS loss and suppress stale turn announcements/rerouting. Confirm recovery when fresh fixes return.
6. Park and test console Wi-Fi loss/reconnection. Confirm fresh video and no lasting pixelation. Test directions through the helmet while phone music is playing.
7. Disconnect explicitly. Confirm the foreground notification disappears and GPS/wake resources stop.

## Evidence

Local logs: Android/data/com.navisarthi.app/files/diag. With adb available: `adb pull /sdcard/Android/data/com.navisarthi.app/files/diag`.

Inspect `fixAge`, `fixGap`, `render`, `encodeSend`, `pending`, frame cadence and encoder recovery events. These measure the phone pipeline; physical console decode/display latency must be observed separately. Never treat a successful UDP send or frame count as proof of displayed frames.

Record results, battery change, maximum temperature/thermal state, visible lag and any freeze duration in the version plan. Keep location logs private. Test each console model/firmware before describing it as supported.
