---
published: true
---

# Battery Monitor

***NB:*** This was done for lenovo **thinkpad T480** but the script can be tweaked for any linux os.

## Problem Statement

I usually forget to plugin my laptop charger either because I have forgotten to remove it from my bag or its charging my phone. Linux notification messages
are small and I do ignore some messages or end up activating **Do Not Disturb** hence not seeing the notification and my pc ends up switching off. This has
nugged me for quite some time and I decided to find a solution to fix that.

## Solution

My solution, checks if battery1 which is the external battery is low. If it is below 10% it ends up suspending the pc and if its below 20 shows you the below alert dialog window.

![zenity-pop-up](https://user-images.githubusercontent.com/70489395/173535627-c180d2ed-d112-48d2-ad5c-896e01f5c774.png)

I would prefer such a pop up alert dialog screen. This is something I can't miss not ignore.

Found a [bash script](https://unix.stackexchange.com/a/227222) which I ended up editing abit.

```sh
#!/usr/bin/env bash

BATTERY=$(upower -e | grep 'BAT1')

while true
do
  BATTERY_PERCENTAGE=$(upower -i "$BATTERY" | grep percentage | awk '{ print $2 }'| sed s/'%'/''/g)
  CABLE=$(upower -i /org/freedesktop/UPower/devices/line_power_AC | grep -n2 line-power | grep online | awk '{ print $3 }')

  if [[ "$BATTERY_PERCENTAGE" -lt "10" && $CABLE = "no" ]]; then

    pidof zenity | xargs kill -9
    notify-send --urgency=critical "WARNING: Battery is about to die"  "Suspending PC ...."
    sleep 10
    systemctl suspend
  
  elif [[ "$BATTERY_PERCENTAGE" -lt "20" && $CABLE = "no" ]]; then

    pidof zenity | xargs kill -9
    sleep 5
    zenity --error --text="Battery Low, please charge device\!" --title="Warning"
  
  fi

  sleep 40

done
```

Below scripts starts up the main script when pc boots up, which can also be called from the terminal.

```sh
#!/usr/bin/env bash

if [[ -n "$(pgrep -f noGoFF.sh)" ]]; then
  exit
else
  noGoFF.sh&
fi
```

To set it up, clone the [repository](https://github.com/iAmG-r00t/whackyscripts), enter the directory, copy all the *.sh* files to `/usr/bin/` directory and link the `battMonitor.sh` file to `autostart` directory.

```sh
git clone https://github.com/iAmG-r00t/whackyscripts.git
cd whackyscripts/battMonitor
sudo cp *.sh /usr/bin/
ln -s /usr/bin/battMonitor.sh ~/.config/autostart/
```

You are all set to go. Do enjoy 😄.
