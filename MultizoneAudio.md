# How to achieve MultizoneAudio in RPI 4B using 2 HDMIs using as Primary Display (HDMI0 as Driver Zone) & Secondary Display (HDMI1 as Rear Zone) running with AOSP14


## Below are the steps followed
1. Overwrite the display port 1 configured as instrument cluster to main display for this PoC in the overlay resource configuration file (overlay/CarServiceRpiOverlay/res/values/config.xml)
2. Remove the cluster packages (ClusterHomeSample / ClusterOsDouble / DirectRenderingCluster) as didn't used for this Multizone Audio PoC
3. Configure the display settings xml file (display_settings.xml) & display layout configuration xml file (display_layout_configuration.xml)
4. Add the "MultiDisplaySecondaryHomeTestLauncher" to the packages to have a launcher for the rear zone display (HDMI1)
5. Configure the car audio zone configuration xml file (car_audio_configuration.xml)
6. Configure the audio policy configuration xml file (audio_policy_configuration.xml)
7. Enable dynamic audio routing feature in the configuration xml file (audioUseDynamicRouting)
8. Enable the focus per display (config_perDisplayFocusEnabled)
9. Increase the maximum multi-users to 10 in the configuration xml file (config_multiuserMaximumUsers)
10. Configure the multi users maximum running users to 5 or more (config_multiuserMaxRunningUsers)
11. Configure to visible the multi user visibility (config_multiuserVisibleBackgroundUsers)
12. Enable the multi user UI (config_enableMultiUserUI)
13. Here used "LocalMediaPlayer" to play the audio concurrently in both the zones. So, "LocalMediaPlayer" is to be configured to support multi instances to run for each users. Refer to "AndroidManifest.xml" for sample configuration usage
14. Configure the Car occupant zones in the configuration xml file

    **config_occupant_zones:**
    ```
    <string-array translatable="false" name="config_occupant_zones">
        <item>occupantZoneId=0,occupantType=DRIVER,seatRow=1,seatSide=driver</item>
        <item>occupantZoneId=1,occupantType=REAR_PASSENGER,seatRow=2,seatSide=right</item>
    </string-array>
    ```
15. Map the display to Car occupant zones in the configuration xml file

    **config_occupant_display_mapping:**
    ```
    <string-array translatable="false" name="config_occupant_zones">
        <item>displayPort=0,displayType=MAIN,occupantZoneId=0</item>
        <item>displayPort=1,displayType=MAIN,occupantZoneId=1</item>
    </string-array>
    ```
16. Enable the user profile assignment for multi display in the configuration xml file (enableProfileUserAssignmentForMultiDisplay)


## Prerequisites
Follow the procedure for cloning & building the repo as mentioned in the link(https://github.com/raspberry-vanilla/android_local_manifest/tree/android-14.0) (Tag : android-14.0.0_r67)

Use the lunch command for building the Car Image ("lunch aosp_rpi4_car-ap2a-userdebug")


## Patches for reference
1. Project "device/brcm/rpi4/" :

    [0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch](./MultizoneAudioPatches/device/brcm/rpi4/0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch)

2. Project "frameworks/base/" :

    [0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch](./MultizoneAudioPatches/frameworks/base/0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch)

3. Project "packages/apps/Car/Launcher/" :

    [0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch](./MultizoneAudioPatches/packages/apps/Car/Launcher/0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch)

4. Project "packages/apps/Car/LocalMediaPlayer/" :

    [0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch](./MultizoneAudioPatches/packages/apps/Car/LocalMediaPlayer/0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch)

5. Project "packages/services/Car/" :

    [0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch](./MultizoneAudioPatches/packages/services/Car/0001-MultizoneAudio-PoC-using-2-HDMI-Ports-Primary-Second.patch)


## Reference Images When Raspberry pi 4B is booted with the above changes
1. Initial Screen :

    [InitialScreen.png](./MultizoneAudioPatches/ReferenceImages/InitialScreen.png)

2. Secondary screen launcher after adding a new profile using "Add a profile" option in the initial bootup screen :

    [SecondaryLauncherScreen.png](./MultizoneAudioPatches/ReferenceImages/SecondaryLauncherScreen.png)

3. Playing audio using LocalMediaPlayer Concurrently in both screens :

    [LaunchingLocalMediaPlayerBothScreens.png](./MultizoneAudioPatches/ReferenceImages/LaunchingLocalMediaPlayerBothScreens.png)


## Note
For some of the xml configuration files changes, overlays can also written but this reference patch modifies directly


## Vote of thanks
Thanks to the author and contributor of the "[Raspberry - Vanilla Github repo](https://github.com/raspberry-vanilla/android_local_manifest/tree/android-14.0)" for having the repo to have quick integration of android into Raspberry Pi and also for the supporting the queries
