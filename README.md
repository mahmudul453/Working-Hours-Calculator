# Working Hours Calculator (Android)

Offline attendance and working-hours tracker for office employees.
Kotlin · Jetpack Compose (Material 3) · Room · MVVM · min Android 8.0 (API 26).

## Features

| Area | What it does |
|---|---|
| Dashboard | Today's date, In/Out, total today, week total, month total, working days, overtime, progress bars, this-vs-last month |
| Attendance | Month calendar (worked / holiday / absent / off-day colours, hours in each cell, tap to add or edit) and a Records list with search by date, month or notes |
| Entry | Android date picker + time pickers, optional break (minutes), notes, live total, overnight shifts, validation, edit and delete (with confirmation), one record per date |
| Weekly report | Monday to Sunday, previous / next week, total, working days, average, overtime, shortfall, weekly progress |
| Monthly report | Any month and year, every date listed, total, working days, average, total break, overtime, shortfall, 6-month comparison |
| Export | CSV and PDF for any week or month (save to phone or share) |
| Backup | JSON backup and restore (records and settings) |
| Settings | Working weekdays, daily / weekly / monthly targets, default break, 12 / 24-hour time, Light / Dark / System theme |

### How hours are calculated

`Worked = (Office Out − Office In) − Break`

If Out is earlier than In the shift crosses midnight, so 10:00 PM → 6:00 AM = 8 h.
Overnight shifts are counted on the date they start. In = Out is rejected, and so is a break as long as the shift.
Overtime / shortfall are measured per working day against the daily target.

## Get the APK

### Option A: Build in the cloud (no Android Studio needed)

1. Create a free GitHub account and a new repository.
2. Upload everything in this folder (or `git push` it), including the hidden `.github` folder.
3. Open the **Actions** tab, choose **Build APK**, and wait about 5 minutes.
   (It also runs on every push; use **Run workflow** to start it manually.)
4. Open the finished run and download the **WorkingHoursCalculator-apk** artifact (a zip).
5. Unzip it. `app-debug.apk` is the safest one to install; `app-release-*.apk` is smaller.

### Option B: Android Studio

1. Install Android Studio (Ladybug 2024.2 or newer).
2. **File → Open** and choose this folder; let Gradle sync finish.
3. **Build → Build APK(s)**. The file is created at `app/build/outputs/apk/debug/app-debug.apk`.

### Option C: Command line

```bash
./gradlew assembleDebug        # needs JDK 17+ and the Android SDK (ANDROID_HOME)
# → app/build/outputs/apk/debug/app-debug.apk
```

## Install the APK on your phone

1. Copy the APK to the phone (USB, Google Drive, WhatsApp to yourself, etc.).
2. Tap the file. If Android asks, allow **Install unknown apps** for the app you opened it from (Files / Chrome / Drive).
3. Tap **Install**, then **Open**.

Data lives only on the phone. Use **Settings → Backup** before changing phones or uninstalling the app.

## Project layout

```
app/src/main/java/com/workinghours/calculator/
  MainActivity.kt            entry point
  data/   Database.kt        Room entity, DAO, database
          Settings.kt        SharedPreferences-backed settings (StateFlow)
          Stats.kt           weekly / monthly summaries, overtime, shortfall
  util/   TimeCalc.kt        all time maths and formatting
          Exporter.kt        CSV, PDF, JSON backup / restore
  ui/     MainViewModel.kt   state + database operations (MVVM)
          App.kt             bottom navigation, export / save / share dialogs
          DashboardScreen, AttendanceScreen (calendar + records), WeeklyScreen,
          MonthlyScreen, SettingsScreen, EntryEditor, Components, ReportParts
          theme/Theme.kt     blue Material 3 light and dark colour schemes
```

## Notes

- The release APK is signed with the debug key so it installs directly. Create your own keystore before publishing on Google Play.
- Holiday and Absent are recorded per date from the entry screen; days without any record are simply "no entry".
