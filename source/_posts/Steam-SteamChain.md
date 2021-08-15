---
title: Steam and SteamChina share the same game file
abbrlink: 611c382f
date: 2021-02-14 14:28:41
tags:
---

# Steam and SteamChina share the same game file

**Recently, SteamChina was launched in China. I download it and want to play the CSGO in the China server. But it reminded me download the game file, I found them don't use the same file. Here is the solution to the problem.**

1. Find the absolute paths about Steam and SteamChina

   ```cmd
   Steam: 			E:\Install\Steam\steamapps
   SteamChina:	E:\Install\SteamChina\steamapps
   ```

2. Delete or rename the folder steamapps in the SteamChina

3. Open the CMD

4. Create the file link

   ```cmd
   mklink /d "E:\Install\SteamChina\steamapps" "E:\Install\Steam\steamapps"
   ```

5. Reload the SteamChina
