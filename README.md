##
#
# GMBRom instalacja
#
##
ui_print(" ");
ui_print("*             -WELCOME            ");
ui_print(" ");
ui_print("*              -WITAM             ");
ui_print(" ");

ui_print("@checking if device is a SGS3 GT-I9300");
assert(getprop("ro.product.device") == "m0" || getprop("ro.build.product") == "m0" || 
       getprop("ro.product.device") == "i9300" || getprop("ro.build.product") == "i9300" || 
       getprop("ro.product.device") == "GT-I9300" || getprop("ro.build.product") == "GT-I9300");
       
ui_print(" - unmounting system partition");
unmount("/cache");
unmount("/data");
unmount("/system");
mount("ext4", "EMMC", "/dev/block/mmcblk0p9", "/system");
mount("ext4", "EMMC", "/dev/block/mmcblk0p8", "/cache");
mount("ext4", "EMMC", "/dev/block/mmcblk0p12", "/data");

show_progress(1.0, 5010);

########################### EFS Backup ##############################

package_extract_file("gm/scripts/efs_backup.sh", "/tmp/efs_backup.sh");
set_perm(0, 0, 0755, "/tmp/efs_backup.sh");
run_program("/tmp/efs_backup.sh");


####----Wipe----####

# Clear Data
	  if file_getprop("/tmp/aroma/clear.prop","selected.1") == "1"
      then
		ui_print("@Clearing Cache and Dalvik...");
		delete_recursive("/cache");
		delete_recursive("/data/dalvik-cache");
		delete_recursive("/dalvik/dalvik-cache");	  
		ui_print("Super Wiping Data...");
		delete_recursive("/system");
		delete_recursive("/data");
		delete_recursive("/preload");
	endif;
	
	  if file_getprop("/tmp/aroma/clear.prop","selected.1") == "2"
      then
		ui_print("@Clearing Cache and Dalvik...");
		delete_recursive("/cache");
		delete_recursive("/data/dalvik-cache");
		delete_recursive("/dalvik/dalvik-cache");	  
		ui_print("Wiping Data...");
		delete_recursive("/system");
		package_extract_file("gm/scripts/wipe.sh", "/tmp/wipe.sh");
		package_extract_file("gm/scripts/bash", "/tmp/bash");
		set_perm(0, 0, 0777, "/tmp/wipe.sh");
		set_perm(0, 0, 0777, "/tmp/bash");
		run_program("/tmp/wipe.sh");
		delete_recursive("/preload");
	endif;
	
# Clear Data
	  if file_getprop("/tmp/aroma/clear.prop","selected.1") == "3"
      then
	  ui_print("@Clearing Cache and Dalvik...");
	  delete_recursive("/cache");
	  delete_recursive("/data/dalvik-cache");
	  delete_recursive("/dalvik/dalvik-cache");
	endif;



ui_print(" ");
ui_print("*        -Installation ROM Grisza Monster Beam");
ui_print(" ");

ui_print("@Installing new system");
package_extract_dir("system", "/system");
package_extract_dir("data", "/data");

ui_print(" - creating symlinks");
symlink("toolbox", "/system/bin/rmdir");
symlink("toolbox", "/system/bin/lsof");
symlink("toolbox", "/system/bin/setprop");
symlink("toolbox", "/system/bin/id");
symlink("toolbox", "/system/bin/reboot");
symlink("toolbox", "/system/bin/nandread");
symlink("toolbox", "/system/bin/playback");
symlink("toolbox", "/system/bin/start");
symlink("toolbox", "/system/bin/cmp");
symlink("toolbox", "/system/bin/uptime");
symlink("toolbox", "/system/bin/route");
symlink("toolbox", "/system/bin/date");
symlink("toolbox", "/system/bin/hd");
symlink("toolbox", "/system/bin/netstat");
symlink("toolbox", "/system/bin/rmmod");
symlink("toolbox", "/system/bin/chmod");
symlink("toolbox", "/system/bin/ps");
symlink("toolbox", "/system/bin/kill");
symlink("toolbox", "/system/bin/touchinput");
symlink("toolbox", "/system/bin/ls");
symlink("toolbox", "/system/bin/touch");
symlink("toolbox", "/system/bin/cat");
symlink("toolbox", "/system/bin/rm");
symlink("toolbox", "/system/bin/setconsole");
symlink("toolbox", "/system/bin/log");
symlink("toolbox", "/system/bin/renice");
symlink("toolbox", "/system/bin/sync");
symlink("toolbox", "/system/bin/dmesg");
symlink("toolbox", "/system/bin/vmstat");
symlink("toolbox", "/system/bin/chown");
symlink("toolbox", "/system/bin/umount");
symlink("toolbox", "/system/bin/mount");
symlink("toolbox", "/system/bin/stop");
symlink("toolbox", "/system/bin/ioctl");
symlink("toolbox", "/system/bin/insmod");
symlink("toolbox", "/system/bin/md5");
symlink("toolbox", "/system/bin/df");
symlink("toolbox", "/system/bin/ln");
symlink("toolbox", "/system/bin/wipe");
symlink("toolbox", "/system/bin/lsmod");
symlink("toolbox", "/system/bin/sleep");
symlink("toolbox", "/system/bin/schedtop");
symlink("toolbox", "/system/bin/printenv");
symlink("toolbox", "/system/bin/smd");
symlink("toolbox", "/system/bin/watchprops");
symlink("toolbox", "/system/bin/notify");
symlink("toolbox", "/system/bin/iftop");
symlink("toolbox", "/system/bin/top");
symlink("toolbox", "/system/bin/ionice");
symlink("toolbox", "/system/bin/sendevent");
symlink("toolbox", "/system/bin/mv");
symlink("mksh", "/system/bin/sh");
symlink("toolbox", "/system/bin/getevent");
symlink("toolbox", "/system/bin/dd");
symlink("toolbox", "/system/bin/newfs_msdos");
symlink("toolbox", "/system/bin/mkdir");
symlink("toolbox", "/system/bin/getprop");
symlink("toolbox", "/system/bin/ifconfig");

ui_print(" - settings system permision");
set_perm_recursive(0, 0, 0755, 0644, "/system");
set_perm_recursive(0, 0, 0755, 0755, "/system/etc/init.d");
set_perm_recursive(0, 2000, 0755, 0755, "/system/bin");
set_perm(0, 3003, 06755, "/system/bin/ip");
set_perm(0, 3003, 02750, "/system/bin/netcfg");
set_perm(0, 3004, 02755, "/system/bin/ping");    #100
set_perm(0, 2000, 06750, "/system/bin/run-as");
set_perm_recursive(1002, 1002, 0755, 0440, "/system/etc/bluetooth");
set_perm(0, 0, 0755, "/system/etc/bluetooth");
set_perm(1000, 1000, 0640, "/system/etc/bluetooth/auto_pairing.conf");
set_perm(3002, 3002, 0444, "/system/etc/bluetooth/blacklist.conf");
set_perm(1002, 1002, 0440, "/system/etc/dbus.conf");
set_perm(1014, 2000, 0550, "/system/etc/dhcpcd/dhcpcd-run-hooks");
set_perm(0, 2000, 0550, "/system/etc/init.goldfish.sh");
set_perm_recursive(0, 0, 0755, 0555, "/system/etc/ppp");
set_perm_recursive(0, 2000, 0755, 0644, "/system/vendor");
set_perm_recursive(0, 2000, 0755, 0644, "/system/vendor/etc");
set_perm_recursive(0, 0, 0755, 0644, "/system/vendor/firmware");
set_perm(0, 2000, 0755, "/system/vendor/firmware");
set_perm(0, 2000, 0755, "/system/vendor/lib");
set_perm_recursive(0, 2000, 0755, 0755, "/system/xbin");
set_perm(0, 1000, 0755, "/system/xbin/busybox");

ui_print(" - busybox");
symlink("/system/xbin/busybox", "/system/bin/busybox");
package_extract_file("setup/sotmax/installbusybox", "/tmp/installbusybox");
set_perm(0, 0, 0777, "/tmp/installbusybox");
run_program("/tmp/installbusybox");
delete("/tmp/installbusybox");

ui_print(" - configuracja root");
set_perm(0, 0, 06755, "/system/xbin/su");
symlink("/system/xbin/su", "/system/bin/su");


ui_print(" - create /data/app subtree");
package_extract_dir("data", "/data");
delete("/data/app/placeholder");

ui_print(" - settings data permision");
set_perm_recursive(1000, 1000, 0771, 0644, "/data/app");


ui_print("@Installing System APP");

if file_getprop("/tmp/aroma/system.prop","item.0.1") == "1"  #
  then
    ui_print(" - AccuweatherWidget");
    package_extract_dir("gm/SystemApp/AccuweatherWidget", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.2") == "1"
  then
    ui_print(" - Allshare");
    package_extract_dir("gm/SystemApp/Allshare", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.3") == "1"
  then
    ui_print(" - Dropbox");
    package_extract_dir("gm/SystemApp/Dropbox", "/system");
endif;
 
if file_getprop("/tmp/aroma/system.prop","item.0.4") == "1"
  then
    ui_print(" - Google now ");
    package_extract_dir("gm/SystemApp/GoogleNow", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.5") == "1"
  then
    ui_print(" - Samsung TTS ");
    package_extract_dir("gm/SystemApp/SamsungTTS", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.6") == "1"
  then
    ui_print(" - FM radio");
    package_extract_dir("gm/SystemApp/FMRadio", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.7") == "1"
  then
    ui_print(" - Google TTS");
    package_extract_dir("gm/SystemApp/GoogleTTS", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.8") == "1"
  then
    ui_print(" - GtoupCast");
    package_extract_dir("gm/SystemApp/GroupCast", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.9") == "1"
  then
    ui_print(" - Kies");
    package_extract_dir("gm/SystemApp/Kies", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.10") == "1"
  then
    ui_print(" - Kies wifi");
    package_extract_dir("gm/SystemApp/kieswifi", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.11") == "1"
  then
    ui_print(" - live wallpaper ");
    package_extract_dir("gm/SystemApp/Live_Wallpapers", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.12") == "1"
  then
    ui_print(" - PaperArtist");
    package_extract_dir("gm/SystemApp/PaperArtist", "/system");
endif;
#300
if file_getprop("/tmp/aroma/system.prop","item.0.13") == "1"
  then
    ui_print(" - PolarisViewer ");
    package_extract_dir("gm/SystemApp/PolarisViewer", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.14") == "1"
  then
    ui_print(" - SCloud ");
    package_extract_dir("gm/SystemApp/S_Cloud", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.15") == "1"
  then
    ui_print(" - Konto samsung ");
    package_extract_dir("gm/SystemApp/Samsung_Account", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.16") == "1"
  then
    ui_print(" - Samsung push serwis ");
    package_extract_dir("gm/SystemApp/Samsung_Push_Service", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.17") == "1"
  then
    ui_print(" - Samsung Apps ");
    package_extract_dir("gm/SystemApp/SamsungApps", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.18") == "1"
  then
    ui_print(" - Email samsunga ");
    package_extract_dir("gm/SystemApp/Samsungmail", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.19") == "1"
  then
    ui_print(" - SimpleWidget ");
    package_extract_dir("gm/SystemApp/Simple_Widgets", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.20") == "1"
  then
    ui_print(" - SMemo");
    package_extract_dir("gm/SystemApp/SMemo", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.21") == "1"
  then
    ui_print(" - STK");
    package_extract_dir("gm/SystemApp/Stk", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.22") == "1"
  then
    ui_print("- Talk");
    package_extract_dir("gm/SystemApp/Talk", "/system");
endif;


if file_getprop("/tmp/aroma/system.prop","item.0.23") == "1"
  then
    ui_print(" - VoiceTalk");
    package_extract_dir("gm/SystemApp/voicetalk", "/system");
endif;


if file_getprop("/tmp/aroma/system.prop","item.0.24") == "1"
  then
    ui_print(" - popup browser");
    package_extract_dir("gm/SystemApp/popup_browser", "/system");
endif;

if file_getprop("/tmp/aroma/system.prop","item.0.25") == "1"
  then
    ui_print(" - Youtube");
    package_extract_dir("gm/SystemApp/youtube", "/system");
endif;

ui_print("@Installing Data App");


if file_getprop("/tmp/aroma/dataapp.prop","item.0.1") == "1"
  then
    ui_print(" - Light menager");
    package_extract_dir("gm/dataapp/light", "/data");
endif;


if file_getprop("/tmp/aroma/dataapp.prop","item.0.2") == "1"
  then
    ui_print(" - Flash Player");
    package_extract_dir("gm/dataapp/flashplayer", "/data");
endif;


if file_getprop("/tmp/aroma/dataapp.prop","item.0.3") == "1"
  then
    ui_print(" - Xposed (per app dpi)");
    package_extract_dir("gm/dataapp/xposed", "/data");
endif;

ui_print(" ");
ui_print("@Installing Keybord...");



if file_getprop("/tmp/aroma/key.prop","selected.1") == "1"
  then
    ui_print(" - Stock keybord");
    package_extract_dir("gm/key/stock", "/system");
endif;


if file_getprop("/tmp/aroma/key.prop","selected.1") == "2"
  then
    ui_print(" - Note II keybord");
    package_extract_dir("gm/key/note2", "/system");
endif;

if file_getprop("/tmp/aroma/key.prop","selected.2") == "1"
  then
    ui_print(" - Xperia T keybord");
    package_extract_dir("gm/key/XperiaT", "/system");
endif;

if file_getprop("/tmp/aroma/key.prop","selected.3") == "1"
  then
    ui_print(" - Android 4.2 keybord");
    package_extract_dir("gm/key/42keybord", "/system");
endif;


ui_print(" ");
ui_print("@Installing Camera...");


if file_getprop("/tmp/aroma/camera.prop","selected.1") == "1"
  then
    ui_print(" - Stock camera");
    package_extract_dir("gm/camera/Stock", "/system");
endif;


if file_getprop("/tmp/aroma/camera.prop","selected.1") == "2"
  then
    ui_print(" - Note II camera (Jobnik)");
    package_extract_dir("gm/camera/notestock", "/system");
endif;


if file_getprop("/tmp/aroma/camera.prop","selected.1") == "3"
  then
    ui_print(" - Note II camera (AOSP theme)");
    package_extract_dir("gm/camera/aospnote", "/system");
endif;


if file_getprop("/tmp/aroma/camera.prop","selected.2") == "1"
  then
    ui_print(" - Android 4.2 camera");
    package_extract_dir("gm/camera/42camera", "/system");
endif;



ui_print(" ");
ui_print("@Installing Launcher...");


if file_getprop("/tmp/aroma/launcher.prop","selected.1") == "1"
  then
    ui_print(" - Stock Touchwiz");
    package_extract_dir("gm/launcher/TW/stock", "/system");
  

  if file_getprop("/tmp/aroma/touchwiz.prop","selected.0") == "1" 
    then
		   ui_print("- No text icon, No scr");
		   ui_print("Extracting files...");
		   package_extract_dir("gm/launcher/TWmod/tw_noscrw_notexticons/vrtheme", "/data/media/vrtheme");
		   set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	 	   set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
		   set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
		   set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
		   ui_print("Installing...");
		   run_program("/data/media/vrtheme/installtheme.sh");
		   ui_print("Cleaning...");
		   run_program("/data/media/vrtheme/cleanup.sh");
		   delete_recursive("/data/media/vrtheme");
		   delete_recursive("/data/media/vrtheme-backup");
  endif;

  if file_getprop("/tmp/aroma/touchwiz.prop","selected.0") == "2" 
    then
		   ui_print(" - No text icon,  scr wall");
		   ui_print("Extracting files...");
		   package_extract_dir("gm/launcher/TWmod/tw_scrw_notexticons/vrtheme", "/data/media/vrtheme");
		   set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
		   set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	 	   set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
		   set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
		   ui_print("Installing...");
		   run_program("/data/media/vrtheme/installtheme.sh");
		   ui_print("Cleaning...");
		   run_program("/data/media/vrtheme/cleanup.sh");
		   delete_recursive("/data/media/vrtheme");
		   delete_recursive("/data/media/vrtheme-backup");
  endif;


  if file_getprop("/tmp/aroma/touchwiz.prop","selected.0") == "3" 
    then
		   ui_print(" - scr wall, text icon");
		   ui_print("Extracting files...");
		   package_extract_dir("gm/launcher/TWmod/tw_scrw_texticons/vrtheme", "/data/media/vrtheme");
		   set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	  	 set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	   	 set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	  	 set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	  	 ui_print("Installing...");
	  	 run_program("/data/media/vrtheme/installtheme.sh");
  		 ui_print("Cleaning...");
	  	 run_program("/data/media/vrtheme/cleanup.sh");
	  	 delete_recursive("/data/media/vrtheme");
	  	 delete_recursive("/data/media/vrtheme-backup");
  endif;
endif;


if file_getprop("/tmp/aroma/launcher.prop","selected.1") == "2"
  then
    ui_print(" - Touchwiz 4x5");
    package_extract_dir("gm/launcher/TW/4x5-4x5", "/system");
  

  if file_getprop("/tmp/aroma/touchwiz45.prop","selected.0") == "1" 
    then
	  	 ui_print(" - No text icon, No scr");
  		 ui_print("Extracting files...");
  		 package_extract_dir("gm/launcher/TWmod/tw_45.45_noscrw_notexticons/vrtheme", "/data/media/vrtheme");
    	 set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
  		 set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	  	 set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
  		 set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
  		 ui_print("Installing...");
  		 run_program("/data/media/vrtheme/installtheme.sh");
  		 ui_print("Cleaning...");
	   	 run_program("/data/media/vrtheme/cleanup.sh");
  		 delete_recursive("/data/media/vrtheme");
	  	 delete_recursive("/data/media/vrtheme-backup"); 
  endif;


  if file_getprop("/tmp/aroma/touchwiz45.prop","selected.0") == "2" 
    then
  		 ui_print(" - no scr wall, text icon");
	  	 ui_print("Extracting files...");
  		 package_extract_dir("gm/launcher/TWmod/tw_45.45_noscrw_texticons/vrtheme", "/data/media/vrtheme");
  		 set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	  	 set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	  	 set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	  	 set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	  	 ui_print("Installing...");
  		 run_program("/data/media/vrtheme/installtheme.sh");
  		 ui_print("Cleaning...");
  		 run_program("/data/media/vrtheme/cleanup.sh");
  		 delete_recursive("/data/media/vrtheme");
	  	 delete_recursive("/data/media/vrtheme-backup");
  endif;


  if file_getprop("/tmp/aroma/touchwiz45.prop","selected.0") == "3"
  	then
      ui_print(" - No text icon,  scr ON");
  	  ui_print("Extracting files...");
	  	package_extract_dir("gm/launcher/TWmod/tw_45.45_scrw_notexticons/vrtheme", "/data/media/vrtheme");
  		set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
  		set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	  	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
  		set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
  		ui_print("Installing...");
  		run_program("/data/media/vrtheme/installtheme.sh");
  		ui_print("Cleaning...");
  		run_program("/data/media/vrtheme/cleanup.sh");
  		delete_recursive("/data/media/vrtheme");
  		delete_recursive("/data/media/vrtheme-backup");
  endif;
endif;


if file_getprop("/tmp/aroma/launcher.prop","selected.1") == "3"
  then
    ui_print(" - Touchwiz JB domination");
    package_extract_dir("gm/launcher/TW/jb_4x5-4x5", "/system");
endif;


if file_getprop("/tmp/aroma/launcher.prop","selected.2") == "1"
  then
    ui_print(" - Apex launcher");
    package_extract_dir("gm/launcher/apex", "/system");
endif;


if file_getprop("/tmp/aroma/launcher.prop","selected.2") == "2"
  then
    ui_print(" - Nexus 4 Launcher");
    package_extract_dir("gm/launcher/42laucher", "/system");
endif;


if file_getprop("/tmp/aroma/launcher.prop","selected.2") == "3"
  then
    ui_print(" - Nova");  #700
    package_extract_dir("gm/launcher/nova", "/system");
endif;


if file_getprop("/tmp/aroma/launcher.prop","selected.2") == "4"
  then
    ui_print(" - Holo Launcher");
    package_extract_dir("gm/launcher/jblauncher", "/system");
endif;


ui_print("@Installing the modified application");

if file_getprop("/tmp/aroma/modapps.prop","selected.1") == "1"
  then
    ui_print("Installing AOSP browser");
    package_extract_dir("gm/Apps/browser/aosp", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.1") == "2"
  then
    ui_print("Installing Stock browser");
    package_extract_dir("gm/Apps/browser/stock", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.2") == "1"
  then
    ui_print("Installing Gmail black theme");
    package_extract_dir("gm/Apps/Gmail/Black", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.2") == "2"
  then
    ui_print("Installing Stock gmail");
    package_extract_dir("gm/Apps/Gmail/Stock", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.2") == "3"
  then
    ui_print("Installing Mod Gmail");
    package_extract_dir("gm/Apps/Gmail/zip", "/system");
endif;

if file_getprop("/tmp/aroma/modapps.prop","selected.2") == "4"
  then
    ui_print("Installing Gmail from android 4.2");
    package_extract_dir("gm/Apps/Gmail/gmail42", "/system");
endif;

if file_getprop("/tmp/aroma/modapps.prop","selected.3") == "1"
  then
    ui_print("Installing Black theme google play");
    package_extract_dir("gm/Apps/Googleplay/black", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.3") == "2"
  then
    ui_print("Installing Stock google play");
    package_extract_dir("gm/Apps/Googleplay/normalny", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.4") == "1"
  then
    ui_print("Installing AOSP black theme SMS app");
    package_extract_dir("gm/Apps/sms/aosp", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.4") == "2"
  then
    ui_print("Installing Stock SMS app");
    package_extract_dir("gm/Apps/sms/stock", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.5") == "1"
  then
    ui_print("Installing Clock pages from android 4.2");
    package_extract_dir("gm/Apps/clock/42", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.5") == "2"
  then
    ui_print("Installing Stock Clock pages");
    package_extract_dir("gm/Apps/clock/stock", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.6") == "1"
  then
    ui_print("Installing Calendar from android 4.2");
    package_extract_dir("gm/Apps/calendar/42", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.6") == "2"
  then
    ui_print("Installing Stock Calendar");
    package_extract_dir("gm/Apps/calendar/stock", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.7") == "1"
  then
    ui_print("Installing google ears");
    package_extract_dir("gm/Apps/googe_ears", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.8") == "1"
  then
    ui_print("Installing Wallpapers from nexus 4");
    package_extract_dir("gm/Apps/wall/wall42", "/system");
endif;


if file_getprop("/tmp/aroma/modapps.prop","selected.8") == "2"
  then
    ui_print("Installing Stock wallpspers");
    package_extract_dir("gm/Apps/wall/stock", "/system");
endif;

# bestMultiTaskingFix
if file_getprop("/tmp/aroma/script.prop","selected.1") == "1"
  then
	  ui_print("@Installing bestMultiTaskingFix v7");
	  package_extract_dir("gm/skrypty/bestMultiTaskingFix/system", "/system");
	  set_perm(0, 2000, 0777, "/system/etc/init.d/S91bestMultiTaskingFix"); 
	  package_extract_file("snakes/Script/bestMultiTaskingFix/build.sh", "/tmp/build.sh");
	  set_perm(0, 0, 0777, "/tmp/build.sh");
	  run_program("/tmp/build.sh");
endif;


if file_getprop("/tmp/aroma/script.prop","selected.1") == "2"
  then
    ui_print("@Installing Supercharger v6");
    package_extract_dir("gm/skrypty/superchargersystem", "/system");
	  package_extract_dir("gm/skrypty/superchargerdata", "/data");
endif;

if file_getprop("/tmp/aroma/script.prop","selected.2") == "1"
  then
    ui_print("@Installing Thunderbolt");
    package_extract_dir("gm/skrypty/thunderbolt", "/system");
endif;

 
if file_getprop("/tmp/aroma/script.prop","selected.3") == "1"
  then
    ui_print("@Installing GPU Rendering");
    package_extract_dir("gm/skrypty/GPU", "/system");
endif;


if file_getprop("/tmp/aroma/script.prop","selected.4") == "1"
  then
    ui_print("@Installing Zipalign script");
    package_extract_dir("gm/skrypty/zipalign", "/system");
endif;


	# EXT4
if file_getprop("/tmp/aroma/script.prop","selected.5") == "1"
  then
	  ui_print("@  Installing EXT4");
	  package_extract_dir("gm/skrypty/EXT4/additions/ext4", "/tmp/ext4");
	  package_extract_dir("gm/skrypty/EXT4/additions/tune2fs", "/tmp/tune2fs");
	  set_perm(0, 0, 0777, "/tmp/ext4");
	  set_perm(0, 0, 0777, "/tmp/tune2fs");
	  run_program("/tmp/ext4");
endif;

if file_getprop("/tmp/aroma/script.prop","selected.6") == "1"
  then
    ui_print("@Installing Governor tweaks");
    package_extract_dir("gm/skrypty/governor", "/system"); 
endif;



# ================================= Installing build.prop tweaks =================================
		
if file_getprop("/tmp/aroma/build_prop.prop","item.0.1") == "1"
	then
		ui_print("@3G tweak adding in build.prop...");
		package_extract_file("gm/skrypty/build.prop-tweaks/3G_con_tweaks.sh", "/tmp/3G_con_tweaks.sh");
		set_perm(0, 0, 0777, "/tmp/3G_con_tweaks.sh");
		run_program("/tmp/3G_con_tweaks.sh");
		ui_print("Done");
endif;

if file_getprop("/tmp/aroma/build_prop.prop","item.0.2") == "1"
	then
		ui_print(" -Force Launcher into memory tweak adding in build.prop...");
		package_extract_file("gm/skrypty/bulid.prop-tweaks/force_launcher_into_memory.sh", "/tmp/force_launcher_into_memory.sh");
		set_perm(0, 0, 0777, "/tmp/force_launcher_into_memory.sh");
		run_program("/tmp/force_launcher_into_memory.sh");
		ui_print("Done");
endif;
	
	
if file_getprop("/tmp/aroma/build_prop.prop","item.0.3") == "1"
	then
		ui_print("@Net Speed tweak adding in build.prop...");
		package_extract_file("gm/skrypty/build.prop-tweaks/internet_speed.sh", "/tmp/internet_speed.sh");
		set_perm(0, 0, 0777, "/tmp/internet_speed.sh");
		run_program("/tmp/internet_speed.sh");
		ui_print("Done");
endif;
	
if file_getprop("/tmp/aroma/build_prop.prop","item.0.4") == "1"
	then
		ui_print("@Photo and Video Recording tweak adding in build.prop...");
		package_extract_file("gm/skrypty/build.prop-tweaks/photo_videorecording_quality.sh", "/tmp/photo_videorecording_quality.sh");
		set_perm(0, 0, 0777, "/tmp/photo_videorecording_quality.sh");
		run_program("/tmp/photo_videorecording_quality.sh");
		ui_print("Done");
endif;
	
if file_getprop("/tmp/aroma/build_prop.prop","item.0.5") == "1"
	then
		ui_print("@JPG Quality tweak adding in build.prop...");
		package_extract_file("gm/skrypty/build.prop-tweaks/raize_JPG_quality.sh", "/tmp/raize_JPG_quality.sh");
		set_perm(0, 0, 0777, "/tmp/raize_JPG_quality.sh");
		run_program("/tmp/raize_JPG_quality.sh");
		ui_print("Done");
endif;
	
if file_getprop("/tmp/aroma/build_prop.prop","item.0.6") == "1"
	then
		ui_print("@Render UI with GPU tweak adding in build.prop...");
		package_extract_file("gm/skrypty/build.prop-tweaks/render_UI_with_GPU.sh", "/tmp/render_UI_with_GPU.sh");
		set_perm(0, 0, 0777, "/tmp/render_UI_with_GPU.sh");
		run_program("/tmp/render_UI_with_GPU.sh");
		ui_print("Done");
endif;
	
if file_getprop("/tmp/aroma/build_prop.prop","item.0.7") == "1"
	then
		ui_print("@Battery Save tweak adding in build.prop...");
		package_extract_file("gm/skrypty/build.prop-tweaks/save_battery.sh", "/tmp/save_battery.sh");
		set_perm(0, 0, 0777, "/tmp/save_battery.sh");
		run_program("/tmp/save_battery.sh");
		ui_print("Done");
endif;


if file_getprop("/tmp/aroma/build_prop.prop","item.0.8") == "1"
	then
		ui_print("@scrolling Responsiveness tweak adding in build.prop...");
		package_extract_file("gm/skrypty/build.prop-tweaks/scrolling_responsiveness.sh", "/tmp/scrolling_responsiveness.sh");
		set_perm(0, 0, 0777, "/tmp/scrolling_responsiveness.sh");
		run_program("/tmp/scrolling_responsiveness.sh");
		ui_print("Done");
endif;
	
if file_getprop("/tmp/aroma/build_prop.prop","item.0.9") == "1"
	then
		ui_print("@touch Responsiveness tweak adding in build.prop...");
		package_extract_file("gm/skrypty/build.prop-tweaks/touch_responsiveness.sh", "/tmp/touch_responsiveness.sh");
		set_perm(0, 0, 0777, "/tmp/touch_responsiveness.sh");
		run_program("/tmp/touch_responsiveness.sh");
		ui_print("Done");
endif;


if file_getprop("/tmp/aroma/adds.prop","selected.1") == "1"
  then
    ui_print("@Installing Call buttom ");
    package_extract_dir("gm/adds/callbuttoms", "/system");
endif;


if file_getprop("/tmp/aroma/system.prop","selected.2") == "1"
  then
    ui_print("@Innstalling High Sound");
    package_extract_dir("gm/adds/highsound", "/system");
endif;


if file_getprop("/tmp/aroma/nexus.prop","selected.1") == "1"
  then
    ui_print("@Inatalling Nexus 4 icons");
		ui_print(" - Extracting files...");
		package_extract_dir("gm/N4/GNexIcon/vrtheme", "/data/media/vrtheme");
		set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
		set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
		set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
		set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
		ui_print(" - Installing...");
		run_program("/data/media/vrtheme/installtheme.sh");
		ui_print(" - Cleaning...");
		run_program("/data/media/vrtheme/cleanup.sh");
		delete_recursive("/data/media/vrtheme");
		delete_recursive("/data/media/vrtheme-backup");
endif;


if file_getprop("/tmp/aroma/nexus.prop","selected.2") == "1"
  then
    ui_print("@Nexus 4 miusic");
		package_extract_dir("gm/N4/Nexus4", "/system");
endif;

ui_print(" ");
ui_print("@Installing Theme");


if file_getprop("/tmp/aroma/theme.prop","selected.0") == "1"
  then
    ui_print("@Stock Theme");
    package_extract_dir("gm/Themes/stock", "/system");
  

    if file_getprop("/tmp/aroma/stock.prop","selected.1") == "1"
      then
        ui_print(" -Installing toggle 23 Stock green ");
        package_extract_dir("gm/Themes/toogle/Stock_23", "/system");

    
       if file_getprop("/tmp/aroma/stock2.prop","selected.1") == "1"
         then
         		ui_print("Installing 100% transparent pulldown background");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/Statusbar/PulldownTrans/100/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.1") == "2"
         then
         		ui_print("Installing 75% transparent pulldown background");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/Statusbar/PulldownTrans/75/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.1") == "3"
         then
         		ui_print("Installing 50% transparent pulldown background");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/Statusbar/PulldownTrans/50/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.1") == "4"
         then
         		ui_print("Installing 25% transparent pulldown background");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/Statusbar/PulldownTrans/25/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;


       if file_getprop("/tmp/aroma/stock2.prop","selected.1") == "5"
         then
         		ui_print("Installing normal pulldown background");
       endif;
   
       if file_getprop("/tmp/aroma/stock2.prop","selected.2") == "1"
         then
         		ui_print("Installing 100% transparent statusbar");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/Statusbar/SbarTrans/100/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.2") == "2"
         then
         		ui_print("Installing 75% transparent statusbar");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/Statusbar/SbarTrans/75/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.2") == "3"
         then
         		ui_print("Installing 50% transparent stsusbar");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/Statusbar/SbarTrans/50/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.2") == "4"
         then
         		ui_print("Installing 25% transparent statusbar");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/Statusbar/SbarTrans/25/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;


       if file_getprop("/tmp/aroma/stock2.prop","selected.2") == "5"
         then
         		ui_print("Installing normal statusbar");
       endif;


       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "1"
         then
         		ui_print("Installing AOSP Circle battey icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battery_mods/AOSP_circle/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "2"
         then
         		ui_print("Installing circle % blue battery icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battery_mods/circle_%_blue/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "3"
         then
         		ui_print("Installing Circle % green battery icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battety_mods/circle_%_green/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;


       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "4"
         then
         		ui_print("Installing Honeycomb blue battery icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battery_mods/honeycomb_blue/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "5"
         then
         		ui_print("Installing honeycomb green battery icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battery_mods/honeycomb_green/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;


       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "6"
         then
         		ui_print("Installing honeycomb grey battery icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battery_mods/honeycomb_grey/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "7"
         then
         		ui_print("Installing Stock blue battery icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battery_mods/stock_blue/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;


       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "8"
         then
         		ui_print("Installing stock green battery icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battery_mods/Stock_green/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "9"
         then
         		ui_print("Installing stock grey battery icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battery_mods/stock_grey/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;


       if file_getprop("/tmp/aroma/stock2.prop","selected.3") == "10"
         then
         		ui_print("Installing white circle battery icon");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/battery_mods/white_circle/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;


       if file_getprop("/tmp/aroma/stock2.prop","selected.4") == "1"
         then
         		ui_print("Installing Transparent multi windows");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/transparent_MWA/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;


       if file_getprop("/tmp/aroma/stock2.prop","selected.4") == "2"
         then
         		ui_print("Installing stock Multi windows");
       endif;
    endif;

    if file_getprop("/tmp/aroma/stock.prop","selected.1") == "2"
      then
        ui_print(" -Installing Stock toggle");
        package_extract_dir("gm/Themes/toogle/Stock", "/system");
    endif;


       if file_getprop("/tmp/aroma/stock3.prop","selected.1") == "1"
         then
         		ui_print("Installing Transparent multi windows");
         		ui_print("Extracting files ...");
         		package_extract_dir("gm/Themes/transparent_MWA/vrtheme", "/data/media/vrtheme");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	        	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	        	ui_print("Installing...");
	         	run_program("/data/media/vrtheme/installtheme.sh");
	        	ui_print("Cleaning...");
         		run_program("/data/media/vrtheme/cleanup.sh");
         		delete_recursive("/data/media/vrtheme");
	        	delete_recursive("/data/media/vrtheme-backup");
       endif;

       if file_getprop("/tmp/aroma/stock3.prop","selected.1") == "2"
         then
         		ui_print("Installing stock Multi windows");
       endif;
    endif;
endif;

if file_getprop("/tmp/aroma/theme.prop","selected.0") == "2"
  then
    package_extract_dir("gm/Themes/aosp", "/system");
		ui_print("@Installing AOSP theme ");


    if file_getprop("/tmp/aroma/aosp.prop","selected.1") == "1"
      then
        ui_print("Installing Blue black toggle without text");
      else
        package_extract_dir("gm/Themes/aospvrt/vrtheme", "/data/media/vrtheme");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	      ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	      ui_print("Cleaning...");
        run_program("/data/media/vrtheme/cleanup.sh");
        delete_recursive("/data/media/vrtheme");
	      delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp.prop","selected.1") == "2"
      then
        ui_print("Installing Blue black toggle with text");
      else
        package_extract_dir("gm/Themes/aospvrt/vrtheme", "/data/media/vrtheme");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	      ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	      ui_print("Cleaning...");
        run_program("/data/media/vrtheme/cleanup.sh");
        delete_recursive("/data/media/vrtheme");
	      delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp.prop","selected.1") == "3"
      then
        ui_print("Installing JB Domination toggle without text");
      else
        package_extract_dir("gm/Themes/aospvrt/vrtheme", "/data/media/vrtheme");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	      ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	      ui_print("Cleaning...");
        run_program("/data/media/vrtheme/cleanup.sh");
        delete_recursive("/data/media/vrtheme");
	      delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp.prop","selected.1") == "4"
      then
        ui_print("Installing JB Domination toggle with text");
      else
        package_extract_dir("gm/Themes/aospvrt/vrtheme", "/data/media/vrtheme");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	      ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	      ui_print("Cleaning...");
        run_program("/data/media/vrtheme/cleanup.sh");
        delete_recursive("/data/media/vrtheme");
	      delete_recursive("/data/media/vrtheme-backup");
    endif;

    
    if file_getprop("/tmp/aroma/aosp2.prop","selected.1") == "1"
      then
        ui_print("Installing 100% transparent pulldown background");
        ui_print("Extracting files ...");
        package_extract_dir("gm/Themes/Statusbar/PulldownTrans/100/vrtheme", "/data/media/vrtheme");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	      ui_print("Installing...");
	      run_program("/data/media/vrtheme/installtheme.sh");
	      ui_print("Cleaning...");
        run_program("/data/media/vrtheme/cleanup.sh");
        delete_recursive("/data/media/vrtheme");
	      delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.1") == "2"
      then
     		ui_print("Installing 75% transparent pulldown background");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/Statusbar/PulldownTrans/75/vrtheme", "/data/media/vrtheme");
      	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.1") == "3"
      then
        ui_print("Installing 50% transparent pulldown background");
        ui_print("Extracting files ...");
        package_extract_dir("gm/Themes/Statusbar/PulldownTrans/50/vrtheme", "/data/media/vrtheme");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	      ui_print("Installing...");
	      run_program("/data/media/vrtheme/installtheme.sh");
	      ui_print("Cleaning...");
        run_program("/data/media/vrtheme/cleanup.sh");
        delete_recursive("/data/media/vrtheme");
	      delete_recursive("/data/media/vrtheme-backup");
     endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.1") == "4"
      then
        ui_print("Installing 25% transparent pulldown background");
        ui_print("Extracting files ...");
        package_extract_dir("gm/Themes/Statusbar/PulldownTrans/25/vrtheme", "/data/media/vrtheme");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	      ui_print("Installing...");
	      run_program("/data/media/vrtheme/installtheme.sh");
	      ui_print("Cleaning...");
        run_program("/data/media/vrtheme/cleanup.sh");
        delete_recursive("/data/media/vrtheme");
	      delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp2.prop","selected.1") == "5"
      then
        ui_print("Installing normal pulldown background");
    endif;
   
    if file_getprop("/tmp/aroma/stock2.prop","selected.2") == "1"
      then
        ui_print("Installing 100% transparent statusbar");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/Statusbar/SbarTrans/100/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.2") == "2"
      then
     		ui_print("Installing 75% transparent statusbar");
       	ui_print("Extracting files ...");
       	package_extract_dir("gm/Themes/Statusbar/SbarTrans/75/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.2") == "3"
      then
       	ui_print("Installing 50% transparent stsusbar");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/Statisbar/SbarTrans/50/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
       	delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.2") == "4"
      then
     		ui_print("Installing 25% transparent statusbar");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/Statusbar/SbarTrans/25/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp2.prop","selected.2") == "5"
      then
     		ui_print("Installing normal statusbar");
    endif;


    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "1"
      then
     		ui_print("Installing AOSP Circle battey icon");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/battery_mods/AOSP_circle/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "2"
      then
       	ui_print("Installing circle % blue battery icon");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/battery_mods/circle_%_blue/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
      	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "3"
      then
     		ui_print("Installing Circle % green battery icon");
     		ui_print("Extracting files ...");
       	package_extract_dir("gm/Themes/battery_mods/circle_%_green/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "4"
      then
     		ui_print("Installing Honeycomb blue battery icon");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/battery_mods/honeycomb_blue/vrtheme", "/data/media/vrtheme");
	      set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "5"
      then
     		ui_print("Installing honeycomb green battery icon");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/battery_mods/honeycomb_green/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
       	delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "6"
      then
     		ui_print("Installing honeycomb grey battery icon");
     		ui_print("Extracting files ...");
       	package_extract_dir("gm/Themes/battery_mods/honeycomb_grey/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "7"
      then
     		ui_print("Installing Stock blue battery icon");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/battery_mods/stock_blue/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "8"
      then
     		ui_print("Installing stock green battery icon");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/battery_mods/Stock_green/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
       	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
      	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;

    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "9"
      then
     		ui_print("Installing stock grey battery icon");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/battery_mods/stock_grey/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp2.prop","selected.3") == "10"
      then
     		ui_print("Installing white circle battery icon");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/battery_mods/white_circle/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip"); 
       	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp2.prop","selected.4") == "1"
      then
     		ui_print("Installing Transparent multi windows");
     		ui_print("Extracting files ...");
     		package_extract_dir("gm/Themes/transparent_MWA/vrtheme", "/data/media/vrtheme");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/installtheme.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zip");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/cleanup.sh");
	     	set_perm(0, 0, 0755, "/data/media/vrtheme/zipalign");
	     	ui_print("Installing...");
	     	run_program("/data/media/vrtheme/installtheme.sh");
	     	ui_print("Cleaning...");
     		run_program("/data/media/vrtheme/cleanup.sh");
     		delete_recursive("/data/media/vrtheme");
	     	delete_recursive("/data/media/vrtheme-backup");
    endif;


    if file_getprop("/tmp/aroma/aosp2.prop","selected.4") == "2"
      then
     		ui_print("Installing Stock Multi windows");
    endif;
endif;

if file_getprop("/tmp/aroma/theme.prop","selected.0") == "3"
  then
    package_extract_dir("gm/Themes/jkay", "/system");
		ui_print("@JKay delux framework theme ");
endif;

ui_print("@(re)settings system permissions");
set_perm_recursive(0, 0, 0755, 0644, "/system");
set_perm_recursive(0, 0, 0755, 0755, "/system/etc/init.d");
set_perm_recursive(0, 2000, 0755, 0755, "/system/bin");
set_perm(0, 3003, 06755, "/system/bin/ip");
set_perm(0, 3003, 02750, "/system/bin/netcfg");
set_perm(0, 3004, 02755, "/system/bin/ping");
set_perm(0, 2000, 06750, "/system/bin/run-as");
set_perm_recursive(1002, 1002, 0755, 0440, "/system/etc/bluetooth");
set_perm(0, 0, 0755, "/system/etc/bluetooth");
set_perm(1000, 1000, 0640, "/system/etc/bluetooth/auto_pairing.conf");
set_perm(3002, 3002, 0444, "/system/etc/bluetooth/blacklist.conf");
set_perm(1002, 1002, 0440, "/system/etc/dbus.conf");
set_perm(1014, 2000, 0550, "/system/etc/dhcpcd/dhcpcd-run-hooks");
set_perm(0, 2000, 0550, "/system/etc/init.goldfish.sh");
set_perm_recursive(0, 0, 0755, 0555, "/system/etc/ppp");
set_perm_recursive(0, 2000, 0755, 0644, "/system/vendor");
set_perm_recursive(0, 2000, 0755, 0644, "/system/vendor/etc");
set_perm_recursive(0, 0, 0755, 0644, "/system/vendor/firmware");
set_perm(0, 2000, 0755, "/system/vendor/firmware");
set_perm(0, 2000, 0755, "/system/vendor/lib");
set_perm_recursive(0, 2000, 0755, 0755, "/system/xbin");
set_perm(0, 1000, 0755, "/system/xbin/busybox");
set_perm(0, 0, 06755, "/system/xbin/su");

ui_print("@(re)settings data permissions");
set_perm_recursive(1000, 1000, 0771, 0644, "/data/app");



ui_print(" ");
ui_print("@Installing modem...");
ui_print(" ");


if file_getprop("/tmp/aroma/modem.prop","selected.0") == "1" then
ui_print("@Flashing modem LLA ...");
package_extract_file("gm/modem/flash_image", "/tmp/flash_image");
set_perm(0, 0, 0777, "/tmp/flash_image");
assert(package_extract_file("gm/modem/XXELLA/modem.bin", "/tmp/modem.bin"),
       run_program("/tmp/flash_image", "/dev/block/mmcblk0p7", "/tmp/modem.bin"),
       delete("/tmp/modem.bin"));
delete("/tmp/flash_image");
endif;


if file_getprop("/tmp/aroma/modem.prop","selected.0") == "2" then
ui_print("Flashing modem LKC...");
package_extract_file("gm/modem/flash_image", "/tmp/flash_image");
set_perm(0, 0, 0777, "/tmp/flash_image");
assert(package_extract_file("gm/modem/XXELKC/modem.bin", "/tmp/modem.bin"),
       run_program("/tmp/flash_image", "/dev/block/mmcblk0p7", "/tmp/modem.bin"),
       delete("/tmp/modem.bin"));
delete("/tmp/flash_image");
endif;




ui_print(" ");
ui_print("@Installing Kernel...");
ui_print(" ");


if file_getprop("/tmp/aroma/kernel.prop","selected.0") == "1" then
ui_print("@Flashing Siyah1.8.9...");
assert(package_extract_file("gm/kernel/siyah/boot.img", "/tmp/boot.img"),
       write_raw_image("/tmp/boot.img", "/dev/block/mmcblk0p5"),
       delete("/tmp/boot.img"));
endif;

if file_getprop("/tmp/aroma/kernel.prop","selected.0") == "2" then
ui_print("@Flashing stock kernel...");
assert(package_extract_file("gm/kernel/stock/boot.img", "/tmp/boot.img"),
       write_raw_image("/tmp/boot.img", "/dev/block/mmcblk0p5"),
       delete("/tmp/boot.img"));
endif;


if file_getprop("/tmp/aroma/kernel.prop","selected.0") == "3" then
ui_print("@Flashing perseus alpha 32.1...");
assert(package_extract_file("gm/kernel/Perseus/boot.img", "/tmp/boot.img"),
       write_raw_image("/tmp/boot.img", "/dev/block/mmcblk0p5"),
       delete("/tmp/boot.img"));
endif;


if file_getprop("/tmp/aroma/kernel.prop","selected.0") == "4" then

    ui_print(" ");
    ui_print("@Yank555.lu kernel Installing...");
    ui_print(" ");

		ui_print(" - extracting installation files");
		package_extract_dir("setup","/tmp");
		set_perm_recursive(0, 0, 0777, 0777, "/tmp/Yank555.lu");
                               

    assert(ui_print("@Extracting new boot image"),
           package_extract_file("boot.img", "/tmp/boot.img"),
           ui_print(" - Flashing new boot image"),
           write_raw_image("/tmp/boot.img", "/dev/block/mmcblk0p5"),
           ui_print(" - Removing temporary file"),
           delete("/tmp/boot.img"));

		ui_print(" ");
		ui_print("@Script generation");
		ui_print(" ");
		
		ui_print(" - cleaning up /system/etc");
		ui_print("   (Yank555.lu settings files)");
		delete("/system/etc/init.kernel.sh");
		delete("/system/etc/init.hardswap.sh");
		
		ui_print(" - cleaning up /system/etc/init.d");
		ui_print("   (Yank555.lu old settings files)");
		delete("/system/etc/init.d/97usbfastcharge");
		delete("/system/etc/init.d/991swap");
		
		ui_print(" - cleaning up /data");
		ui_print("   (Yank555.lu old settings files)");
		delete("/data/kernel-script.log");
		delete("/data/hardswap.log");
		delete("/data/swap.0.log");
		delete("/data/swap.1.log");
		delete("/data/swap.2.log");
		delete("/data/swap.3.log");
		delete("/data/swap.4.log");
		
		ui_print(" - starting script generator");
		ui_print("   (see /data/kernel-script.log for details)");
		
		run_program("/tmp/Yank555.lu/script/install.sh");
		
		if file_getprop("/tmp/aroma/swap.prop","selected.1") == "2"
		  then
		
		    ui_print(" ");
		    ui_print("@Hardswap installation");
		    ui_print(" ");
		
				ui_print(" - starting installation script");
		    ui_print("   (see /data/hardswap.log for details)");
				run_program("/tmp/Yank555.lu/swap/install/install.sh");
				
				if file_getprop("/tmp/Yank555.lu/swap/install/hardswap.prop","swap.present") == "yes"
				  then
				    ui_print(" - swap partition found on SD-card");
				    ui_print("     '",file_getprop("/tmp/Yank555.lu/swap/install/hardswap.prop","swap.device"),"'");
				    if file_getprop("/tmp/Yank555.lu/swap/install/hardswap.prop","swap.formated") == "yes"
				      then
				        ui_print(" - swap partition formated");
				    endif;
				    if file_getprop("/tmp/Yank555.lu/swap/install/hardswap.prop","swap.scriptinstalled") == "yes"
				      then
				        ui_print(" - swap activation script installed to /system/etc");
				    endif;
				  else
				    ui_print("   Problem, no swap partiton found !");
				    ui_print("   --> Hardswap installation aborted.");
				endif;
		  
		endif;
		

endif;


if file_getprop("/tmp/aroma/kernel.prop","selected.0") == "5" then
ui_print("@Flashing Boeffla kernel 2.7.2...");
assert(package_extract_file("gm/kernel/Boeffla/boot.img", "/tmp/boot.img"),
       write_raw_image("/tmp/boot.img", "/dev/block/mmcblk0p5"),
       delete("/tmp/boot.img"));
endif;


if file_getprop("/tmp/aroma/kernel.prop","selected.0") == "5" then
ui_print("@Flashing HydRx kernel...");


ui_print("");
ui_print("************************************************************");
ui_print("*                                                ");
ui_print("*                       Welcome to HydRx-kernel          ");
ui_print("*                                              ");
ui_print("************************************************************");
ui_print("");
ui_print("");
ui_print("************************************************************");
mount("ext4", "EMMC", "/dev/block/mmcblk0p9", "/system");
mount("ext4", "EMMC", "/dev/block/mmcblk0p8", "/cache");
mount("ext4", "EMMC", "/dev/block/mmcblk0p12", "/data");
delete("/system/etc/init.d/S91voltctrl");
delete("/system/etc/init.d/S98bolt_siyah");
delete_recursive("/sbin/siyah");
delete_recursive("/data/.siyah");
delete_recursive("/system/.siyah");
delete("/system/etc/init.d/01siyah");
delete("/system/etc/init.d/S_volt_scheduler");
delete_recursive("/data/void-settings");
delete("/res/misc/void");
delete("/system/bin/void");
delete("/xbin/bin/void");
delete("/res/misc/NEAK-Downloader.apk");
delete_recursive("/data/neak");
delete("/res/neak.extra");
delete_recursive("/sbin/near");
delete("/system/etc/lionheart");
delete("/system/etc/schedmc");
delete_recursive("/sys/devices/system/cpu/cpu0/cpufreq");
delete_recursive("/sys/devices/system/cpu/cpu/cpufreq");
delete("/sys/devices/system/cpu/cpu/sched_mc_power_savings");
delete_recursive("/system/.androidmeda");
delete_recursive("/data/data/net.fluxi.xxTweaker");
delete_recursive("/data/data/net.fluxi.xxTweak/");
delete("/data/app/net.fluxi.xxTweaker-1.apk");
delete("/data/.notweaker");
delete_recursive("/data/.Abyss/");
delete_recursive("/system/.Abyss/");
delete("/sbin/ext/abyss.sh");
delete("/res/misc/S90abyss");
delete("/data/data/com.teamblockbuster.thoravukk.app");
delete("/data/app/com.teamblockbuster.thoravukk.app");
delete("/system/etc/init.d/thoravukk");
delete("/data/.thoravukk");
delete_recursive("/data/data/com.teamblockbuster");
delete("/system/litepro");
delete("/system/etc/init.d/99nstools");
delete("/data/user.log");
delete("/system/Hawker.u.copier");
delete("/system/bin/simproving");
delete("/system/bin/simproving2");
delete("/system/bin/simproving3");
delete("/system/bin/nowifi");
################################################################
########################## KERNEL ##############################
################################################################
ui_print("*         -Flashing Kernel...              ");
ui_print("@Installing Kernel...");
# HydRx kernel
	if 
	  file_getprop("/tmp/aroma/kernel2.prop","selected.1") == "1"
	then
	  ui_print("*         -Flashing HydRx kernel...");
	  assert(package_extract_file("extras/kernel/hydrx/boot.img", "/tmp/boot.img"),
	  write_raw_image("/tmp/boot.img", "/dev/block/mmcblk0p5"),
	  delete("/tmp/boot.img"));
	endif;

# HydRxtremE kernel
	if 
	  file_getprop("/tmp/aroma/kernel2.prop","selected.1") == "2"
	then
	  ui_print("*         -Flashing HydRxtremE kernel...");
	  assert(package_extract_file("extras/kernel/hydrxtreme/boot.img", "/tmp/boot.img"),
	  write_raw_image("/tmp/boot.img", "/dev/block/mmcblk0p5"),
	  delete("/tmp/boot.img"));
	endif;

################################################################
###################### UNDERVOLT ###############################
################################################################
# HydRx NOUV kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "1"
	then
	  ui_print("*         -HydRx NO-UV default...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/nouv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -25mV kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "2"
	then
	  ui_print("*         -HydRx -25mV...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/25mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRx -50mV kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "3"
	then
	  ui_print("*         -HydRx -50mV...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/50mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -75mV kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "4"
	then
	  ui_print("*         -HydRx -75mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/75mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -75mV kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "5"
	then
	  ui_print("*         -HydRx -75mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/75mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -100mV-a kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "6"
	then
	  ui_print("*         -HydRx -100mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/100mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -100mV-b kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "7"
	then
	  ui_print("*         -HydRx -100mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/100mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -100mV-c kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "8"
	then
	  ui_print("*         -HydRx -100mV-c...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/100mv-c/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -100mV-d kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "9"
	then
	  ui_print("*         -HydRx -100mV-d...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/100mv-d/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRx -125mV-a kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "10"
	then
	  ui_print("*         -HydRx -125mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/125mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -125mV-b kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "11"
	then
	  ui_print("*         -HydRx -125mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/125mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRx -125mV-c kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "12"
	then
	  ui_print("*         -HydRx -125mV-c...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/125mv-c/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -125mV-d kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "13"
	then
	  ui_print("*         -HydRx -125mV-d...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/125mv-d/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -137.5mV-a kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "14"
	then
	  ui_print("*         -HydRx -137.5mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/137.5mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -137.5mV-b kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "15"
	then
	  ui_print("*         -HydRx -137.5mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/137.5mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRx -137.5mV-c kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "16"
	then
	  ui_print("*         -HydRx -137.5mV-c...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/137.5mv-c/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -137.5mV-d kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "17"
	then
	  ui_print("*         -HydRx -137.5mV-d...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/137.5mv-d/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -150mV-a kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "18"
	then
	  ui_print("*         -HydRx -150mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/150mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -150mV-b kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "19"
	then
	  ui_print("*         -HydRx -150mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/150mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRx -150mV-c kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "20"
	then
	  ui_print("*         -HydRx -150mV-c...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/150mv-c/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRx -150mV-d kernel
	if 
	  file_getprop("/tmp/aroma/hydrx.prop","selected.1") == "21"
	then
	  ui_print("*         -HydRx -150mV-d...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/150mv-d/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

################################################################
################################################################
# HydRxtremE NOUV kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "1"
	then
	  ui_print("*         -HydRxtremE NO-UV default...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/nouv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -25mV kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "2"
	then
	  ui_print("*         -HydRxtremE -25mV...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/25mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRxtremE -50mV kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "3"
	then
	  ui_print("*         -HydRxtremE -50mV...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/50mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -75mV kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "4"
	then
	  ui_print("*         -HydRxtremE -75mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/75mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -75mV kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "5"
	then
	  ui_print("*         -HydRxtremE -75mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/75mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -100mV-a kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "6"
	then
	  ui_print("*         -HydRxtremE -100mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/100mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -100mV-b kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "7"
	then
	  ui_print("*         -HydRxtremE -100mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/100mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -100mV-c kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "8"
	then
	  ui_print("*         -HydRxtremE -100mV-c...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/100mv-c/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -100mV-d kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "9"
	then
	  ui_print("*         -HydRxtremE -100mV-d...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/100mv-d/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRxtremE -125mV-a kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "10"
	then
	  ui_print("*         -HydRxtremE -125mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/125mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -125mV-b kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "11"
	then
	  ui_print("*         -HydRxtremE -125mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/125mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRxtremE -125mV-c kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "12"
	then
	  ui_print("*         -HydRxtremE -125mV-c...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/125mv-c/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -125mV-d kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "13"
	then
	  ui_print("*         -HydRxtremE -125mV-d...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/125mv-d/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -137.5mV-a kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "14"
	then
	  ui_print("*         -HydRxtremE -137.5mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/137.5mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -137.5mV-b kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "15"
	then
	  ui_print("*         -HydRxtremE -137.5mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/137.5mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRxtremE -137.5mV-c kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "16"
	then
	  ui_print("*         -HydRxtremE -137.5mV-c...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/137.5mv-c/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -137.5mV-d kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "17"
	then
	  ui_print("*         -HydRxtremE -137.5mV-d...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/137.5mv-d/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -150mV-a kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "18"
	then
	  ui_print("*         -HydRxtremE -150mV-a...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/150mv-a/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -150mV-b kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "19"
	then
	  ui_print("*         -HydRxtremE -150mV-b...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/150mv-b/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;

# HydRxtremE -150mV-c kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "20"
	then
	  ui_print("*         -HydRxtremE -150mV-c...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/150mv-c/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
# HydRxtremE -150mV-d kernel
	if 
	  file_getprop("/tmp/aroma/hydrxtreme.prop","selected.1") == "21"
	then
	  ui_print("*         -HydRxtremE -150mV-d...");
	  delete("/system/bin/simproving");
	  delete("/system/bin/simproving2");
	  delete("/system/bin/simproving3");
	  package_extract_dir("extras/kernel/hydrxuv/150mv-d/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	endif;
	
##############################################################################
# CPU NOUV
	if 
	  file_getprop("/tmp/aroma/cpu2.prop","selected.1") == "1"
	then
	  ui_print("*         -CPU NO-UV default...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/nouv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# CPU -25mV kernel
	if 
	  file_getprop("/tmp/aroma/cpu2.prop","selected.1") == "2"
	then
	  ui_print("*         -CPU -25mV...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/25mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;

# CPU -50mV kernel
	if 
	  file_getprop("/tmp/aroma/cpu2.prop","selected.1") == "3"
	then
	  ui_print("*         -CPU -50mV...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/50mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# CPU -75mV kernel
	if 
	  file_getprop("/tmp/aroma/cpu2.prop","selected.1") == "4"
	then
	  ui_print("*         -CPU -75mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/75mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
		
# CPU -100mV-a kernel
	if 
	  file_getprop("/tmp/aroma/cpu2.prop","selected.1") == "5"
	then
	  ui_print("*         -CPU -100mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/100mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
	
# CPU -125mV-a kernel
	if 
	  file_getprop("/tmp/aroma/cpu2.prop","selected.1") == "6"
	then
	  ui_print("*         -CPU -125mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/125mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# CPU -137.5mV-a kernel
	if 
	  file_getprop("/tmp/aroma/cpu2.prop","selected.1") == "7"
	then
	  ui_print("*         -CPU -137.5mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/137.5mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# CPU -150mV-a kernel
	if 
	  file_getprop("/tmp/aroma/cpu2.prop","selected.1") == "8"
	then
	  ui_print("*         -CPU -150mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/150mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
######################################################################
# GPU NOUV
	if 
	  file_getprop("/tmp/aroma/gpu.prop","selected.1") == "1"
	then
	  ui_print("*         -GPU NO-UV default...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/nouv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# GPU -25mV kernel
	if 
	  file_getprop("/tmp/aroma/gpu.prop","selected.1") == "2"
	then
	  ui_print("*         -GPU -25mV...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/25mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;

# GPU -50mV kernel
	if 
	  file_getprop("/tmp/aroma/gpu.prop","selected.1") == "3"
	then
	  ui_print("*         -GPU -50mV...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/50mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# GPU -75mV kernel
	if 
	  file_getprop("/tmp/aroma/gpu.prop","selected.1") == "4"
	then
	  ui_print("*         -GPU -75mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/75mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
		
# GPU -100mV-a kernel
	if 
	  file_getprop("/tmp/aroma/gpu.prop","selected.1") == "5"
	then
	  ui_print("*         -GPU -100mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/100mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# GPU -125mV-a kernel
	if 
	  file_getprop("/tmp/aroma/gpu.prop","selected.1") == "6"
	then
	  ui_print("*         -GPU -125mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/125mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;	
######################################################################
# INT NOUV
	if 
	  file_getprop("/tmp/aroma/int.prop","selected.1") == "1"
	then
	  ui_print("*         -INT NO-UV default...");
	  package_extract_dir("extras/kernel/hydrxuv/int/nouv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# INT -25mV kernel
	if 
	  file_getprop("/tmp/aroma/int.prop","selected.1") == "2"
	then
	  ui_print("*         -INT -25mV...");
	  package_extract_dir("extras/kernel/hydrxuv/int/25mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;

# INT -50mV kernel
	if 
	  file_getprop("/tmp/aroma/int.prop","selected.1") == "3"
	then
	  ui_print("*         -INT -50mV...");
	  package_extract_dir("extras/kernel/hydrxuv/int/50mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# INT -75mV kernel
	if 
	  file_getprop("/tmp/aroma/int.prop","selected.1") == "4"
	then
	  ui_print("*         -INT -75mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/int/75mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;

# INT -87.5mV kernel
	if 
	  file_getprop("/tmp/aroma/int.prop","selected.1") == "5"
	then
	  ui_print("*         -INT -87.5mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/int/87.5mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
		
# INT -100mV-a kernel
	if 
	  file_getprop("/tmp/aroma/int.prop","selected.1") == "6"
	then
	  ui_print("*         -INT -100mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/int/100mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# INT -112.5mV-a kernel
	if 
	  file_getprop("/tmp/aroma/int.prop","selected.1") == "7"
	then
	  ui_print("*         -INT -112.5mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/int/112.5mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;	
#########################################################################
##############################################################################
# CPU NOUV
	if 
	  file_getprop("/tmp/aroma/cpu1.prop","selected.1") == "1"
	then
	  ui_print("*         -CPU NO-UV default...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/nouv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# CPU -25mV kernel
	if 
	  file_getprop("/tmp/aroma/cpu1.prop","selected.1") == "2"
	then
	  ui_print("*         -CPU -25mV...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/25mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;

# CPU -50mV kernel
	if 
	  file_getprop("/tmp/aroma/cpu1.prop","selected.1") == "3"
	then
	  ui_print("*         -CPU -50mV...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/50mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# CPU -75mV kernel
	if 
	  file_getprop("/tmp/aroma/cpu1.prop","selected.1") == "4"
	then
	  ui_print("*         -CPU -75mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/75mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
		
# CPU -100mV-a kernel
	if 
	  file_getprop("/tmp/aroma/cpu1.prop","selected.1") == "5"
	then
	  ui_print("*         -CPU -100mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/100mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
	
# CPU -125mV-a kernel
	if 
	  file_getprop("/tmp/aroma/cpu1.prop","selected.1") == "6"
	then
	  ui_print("*         -CPU -125mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/125mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# CPU -137.5mV-a kernel
	if 
	  file_getprop("/tmp/aroma/cpu1.prop","selected.1") == "7"
	then
	  ui_print("*         -CPU -137.5mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/137.5mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# CPU -150mV-a kernel
	if 
	  file_getprop("/tmp/aroma/cpu1.prop","selected.1") == "8"
	then
	  ui_print("*         -CPU -150mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/150mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
######################################################################
# GPU NOUV
	if 
	  file_getprop("/tmp/aroma/gpu1.prop","selected.1") == "1"
	then
	  ui_print("*         -GPU NO-UV default...");
	  package_extract_dir("extras/kernel/hydrxuv/cpu/nouv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# GPU -25mV kernel
	if 
	  file_getprop("/tmp/aroma/gpu1.prop","selected.1") == "2"
	then
	  ui_print("*         -GPU -25mV...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/25mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;

# GPU -50mV kernel
	if 
	  file_getprop("/tmp/aroma/gpu1.prop","selected.1") == "3"
	then
	  ui_print("*         -GPU -50mV...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/50mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# GPU -75mV kernel
	if 
	  file_getprop("/tmp/aroma/gpu1.prop","selected.1") == "4"
	then
	  ui_print("*         -GPU -75mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/75mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
		
# GPU -100mV-a kernel
	if 
	  file_getprop("/tmp/aroma/gpu1.prop","selected.1") == "5"
	then
	  ui_print("*         -GPU -100mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/100mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# GPU -125mV-a kernel
	if 
	  file_getprop("/tmp/aroma/gpu1.prop","selected.1") == "6"
	then
	  ui_print("*         -GPU -125mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/gpu/125mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;	
######################################################################
# INT NOUV
	if 
	  file_getprop("/tmp/aroma/int1.prop","selected.1") == "1"
	then
	  ui_print("*         -INT NO-UV default...");
	  package_extract_dir("extras/kernel/hydrxuv/int/nouv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# INT -25mV kernel
	if 
	  file_getprop("/tmp/aroma/int1.prop","selected.1") == "2"
	then
	  ui_print("*         -INT -25mV...");
	  package_extract_dir("extras/kernel/hydrxuv/int/25mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;

# INT -50mV kernel
	if 
	  file_getprop("/tmp/aroma/int1.prop","selected.1") == "3"
	then
	  ui_print("*         -INT -50mV...");
	  package_extract_dir("extras/kernel/hydrxuv/int/50mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# INT -75mV kernel
	if 
	  file_getprop("/tmp/aroma/int1.prop","selected.1") == "4"
	then
	  ui_print("*         -INT -75mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/int/75mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;

# INT -87.5mV kernel
	if 
	  file_getprop("/tmp/aroma/int1.prop","selected.1") == "5"
	then
	  ui_print("*         -INT -87.5mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/int/87.5mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
		
# INT -100mV-a kernel
	if 
	  file_getprop("/tmp/aroma/int1.prop","selected.1") == "6"
	then
	  ui_print("*         -INT -100mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/int/100mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;
	
# INT -112.5mV-a kernel
	if 
	  file_getprop("/tmp/aroma/int1.prop","selected.1") == "7"
	then
	  ui_print("*         -INT -112.5mV-a...");
	  package_extract_dir("extras/kernel/hydrxuv/int/112.5mv/bin", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/simproving");
	  set_perm(0, 0, 06755, "/system/bin/simproving2");
	  set_perm(0, 0, 06755, "/system/bin/simproving3");
	endif;	
#########################################################################
#########################################################################
# Enable wifi auto-disconnection
	if 
	  file_getprop("/tmp/aroma/wifioff.prop","selected.1") == "1"
	then
	  ui_print("*         -Enabling wifi auto-disconnection...");
	  delete("/system/bin/nowifi");
	endif;
	
# Disable wifi auto-disconnection
	if 
	  file_getprop("/tmp/aroma/wifioff.prop","selected.1") == "2"
	then
	  ui_print("*         -Disabling wifi auto-disconnection...");
	  package_extract_dir("extras/kernel/wifi/disable", "/system/bin");
	  set_perm(0, 0, 06755, "/system/bin/nowifi");
	endif;
#########################################################################
endif;


ui_print(" ");
ui_print("@Cleaning up");
ui_print(" ");

ui_print(" - removing installation files");
delete_recursive(/tmp/Yank555.lu);

show_progress(0.100000, 0);

unmount("/system");
unmount("/cache");
unmount("/data");
unmount("/preload");
ui_print("*         -Installation Finished...        ");
ui_print("***********************************************************");

ui_print("*         -Instalacja Zakonczona...        ");
ui_print("***********************************************************");

ui_print("@Thanks for choosing my ROM");
ui_print(" ");
ui_print("@Dzieki za wybor mojego ROMu");
ui_print(" ");
ui_print("@Greets. Grzesdroid");
ui_print(" ");
ui_print("@Pozdrawiam. Grzesdroid");
