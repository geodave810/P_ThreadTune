# P_ThreadTune
Fine-Tune your Screw Thread Designs in Autodesk Fusion 360

This was formally called **ThreadTune**, but I changed the name of it after finding another repository on github by the same name.

P_ThreadTune starting with v1.5 now works with both Metric & English threads.  If your units for the active drawing is Ft or In, it will default to English.  If it is a metric unit, it will default to Metric threads & expand the appropriate Metric or English thread options on Tab1 & Tab2.  You can still change it to English or Metric within the dialog box & expand or contract the Metric or English options on either tab.

This started out to be just a way to be able to give more adjustable threads by varying the Diameter, Pitch Height and Angle like I can easily do using openSCAD.  This gave me a lot of flexibility with tolerance fit between male & female threads.  The formula for the thread profile is pretty simple to generate a single profile.  I used this page for reference.
https://en.wikipedia.org/wiki/ISO_metric_screw_thread

Here is a forum discussion I started on this idea & shows most of my thoughts along the way.  I won't repeat a lot of the info from there.
https://forum.v1e.com/t/drawing-more-flexible-screw-threads-in-fusion-360/43352

You can create threads with most any angle as well as using different angles for the top thread & bottom thread.  If your combined top & bottom angle are too large, the code will fail.  Using a 30 & 60 will work, but using a 60 & 60 will fail.  If you set either or both angles to 0, it will give you a flat thread on the top, bottom or both.  You can also enter a negative number for the thread angle for the top, bottom or both which will invert the thread.  I did see a YouTube video where the inverted style was actually used.  You can also do a Right or Left hand threads.

How this works is that it draws a profile of one thread for the Helix & Pattern option.  Then it draws a helix one revolution the height of it based on the helix pitch following the inside edge of that profile & another helix along the outside radius of that profile for a Guide Rail.  The Pattern option will give you the most accuracy & much faster than the other 2 options. The more spline points you use the more accurate the thread will be for the full length of the thread.  I have the default currently set to Pattern type & 18 spline points.  For 3D printing, 18 spline points should be accurate enough.  You can also have a Long helix with the same revolutions of the entire screw, but that will be slower & really slow if there the threads are long & I would not recommend it.  The long helix can bog down fusion & even crash it.  I left the Helix & Long Helix in there in case Fusion changes they way these are handled down the road.  I would stick with Pattern option.  The Pattern option draws onethread profile & one revolution of the helix. It then models one revolution of the threads & uses the pattern along a path to create all the threads. It is faster & more accurate because it is simply copying only one revolution of the thread.  The DrawCylinder routine will combine them all together.

Since the thread profiles are on the timeline, you can fine tune these even further by changing the thread profiles even further like changing one or more of the lines in the profile to a curve between the points.  The start of each helix just needs to attach to part of the sweeping thread.

This program is setup as an Add-In in Autodesk Fusion 360.  To install this, copy the file structure to:
C:\Users\<UserName>\AppData\Roaming\Autodesk\Autodesk Fusion 360\API\AddIns
After you copied the files, your folder structure should look like this.<br>
![P_ThreadTuneFolder](https://github.com/geodave810/P_ThreadTune/assets/13069472/588973b7-f680-4212-b6b8-77f84619f901)<br>
To run the program, click on Utilites, then ADD-INS, then Add-Ins tab.  You should see the program ThreadTune in the list under **My Add-Ins**
Click on the program & you should see a thread icon in the ADD-INS panel.  To run the program, just click on that thread icon.

The most common use of this program for me anyway is to have a different diameter for the Male threads & the Female threads in order to get a good tolerance in 3D prints.  For example, using 6mm diameter threads with a pitch of 1 & angle of 30, then a 5.5mm diameter threads with same parameters you will get a tolerance of 0.25mm all the way around the vertical part of the threads, but a little less around the angled part of the threads.<br>
![ThreadTune_v1_5_Tab1M](https://github.com/geodave810/P_ThreadTune/assets/13069472/d6cc5184-a5ed-4ee1-89dc-6520145227ff)
![ThreadTune_v1_5_Tab1E](https://github.com/geodave810/P_ThreadTune/assets/13069472/583bcf6e-67ff-4c92-88e1-0cfb876c6b42)
![ThreadTune_v1_5_Tab2M](https://github.com/geodave810/P_ThreadTune/assets/13069472/687ed454-d04f-427d-aa9c-c8b320676700)
![ThreadTune_v1_5_Tab2E](https://github.com/geodave810/P_ThreadTune/assets/13069472/493f04a5-2827-4dbf-96a9-be419e3a3d40)
<br>
![ThreadTune_TestRun](https://github.com/geodave810/P_ThreadTune/assets/13069472/65711d0e-00a0-4018-bdee-cf31613d7b88)

Using the Helex guide rail instead of the vertical centerline will require less points for the spline points to be accurate.  18 - 36 spline points is usually sufficent with the Helix guiderail, but you will need at least 36-45 spline points when using the vertical centerline for it to be accurate.  Using a different angle for the top & bottom of the thread you will need to use the vertical centerline for it to work.  The exception here is using 0 for the top and/or bottom angle either type of guide rail will work.  If there was a helix you could draw in sketch mode for fusion 360 which there does not seem to be at the moment, the curvature would probably be quite accurate.  AutoCAD seems to have the ability to draw a helix, so maybe it will be added one day.<br>
![P_Thread_Types_800x600](https://github.com/geodave810/P_ThreadTune/assets/13069472/6a7c3e2f-fd54-495e-970b-8786e7d4d83f)
<br>

This program draws the thread at 0,0,0 and only draws the threads.  A cylinder is drawn with inner radius of threads & the threads are cut flush with the top & bottom of the cylinder.  There is a file **DialogInput.txt** that saves your current dialog input parameters to set as the defaults for the next time you use the program.  This file is saved in the same folder as the running program.

I will add the little caveats of this program as I come across them.

1. Since English threads generally require more precision because the way the threads are defined, I ran into a problem if your Preferences Unit and Value Display General Precision is set to a low number like 3. This is because English threads are measured with Threads per Inch (TPI). For instance 24 TPI would be a decimal equivalent of 0.041666667". You might want to consider changing this to 9 places in your preferences. Probably will not cause you a problem either way as I save the current preference value, set it to 9, then change it back to what you set it to after that portion of the code is run.
2. Here are the decimal equivalents of the English standard TPI values I have come across.
TPI<br>
32	   0.031250000<br>
24	   0.041666667<br>
20	   0.050000000<br>
18	   0.055555556<br>
16	   0.062500000<br>
14	   0.071428571<br>
12	   0.083333333<br>
11	   0.090909091<br>
10	   0.100000000<br>
9	     0.111111111<br>
8	     0.125000000<br>
7	     0.142857143<br>
6	     0.166666667<br>
4.5    0.222222222<br>

4. When creating the thread bodies, I do a join of the sweep of the thread profile & the inner cylinder.  If you have another body located where it will join that one also.  I have not gone to the trouble of hiding everything 

5. You can bog the program down if your spline helix has a lot of points for the sweep of the threads.  18 spline points should be sufficient.  If you have too many points, you might get an error dialog.  Use Pattern option for best results & using more spline point up to 45 does not really affect that.  Generally speaking 18 should still give you sufficient accuracy for 3D printing.

6. This will probably work better in a new design.  If you don't, just be sure to hide any objects near origin before running.

7. For long threads, it will occasionally not create the inner cylinder to connect the threads, but with the Pattern option I have created M8x500mm with 1.25mm pitch threads without a problem.  You should only have problems with long threads using the Helix or Long Helix option.  I have occasionally had to restart fusion when something did not work right & after restarting it did.

8. I have locked up fusion & had to kill the process 2 or 3 times, but not since I rewrote the helix drawing method to draw just one revolution of the helix.  I left the long helix method in there, but would not recommend it.  Again, stick with the Pattern option.

9. There is an option for Real offsets which means the offset will be the same gap all the way around.  ChatGPT helped with the code for this & I commented the code where that was added.

If you find this program useful to you, consider making a donation from the link on the right or purchasing one of my audio recordings of Mountain streams from Amazon in the 2nd link on the right.
