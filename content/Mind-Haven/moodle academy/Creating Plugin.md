---
title: Creating Plugin
tags:
  - moodle-academy
  - tutorial
cssclasses: []
---

Make sure to have Moodle plugin skeleton generator installed.

Site Administration > Notification -- trigger plugin installation
Site Administration > Development > Skeleton Generator

![[Pasted image 20241210165545.png]]
![[Pasted image 20241210165553.png]]

Then you can generate plugin, which will download you a .zip file. Unzip and move to your moodle directory under `local` (for now).

Afterwards, go to Site administration to trigger the new plugin installation (you will get errors), after installation, go to Site Administration > Plugins > Plugins overview > Additional plugins and you will see your plugin there.

*Note*: The uninstall process is capable of **removing the source code of your plugin without any backup**. If you choose to remove the plugin folder, Moodle will delete all files for the plugin, so that the plugin does not reinstall itself.