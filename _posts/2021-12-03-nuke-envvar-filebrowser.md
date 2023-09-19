---
title: "Nuke File Browser environment variable"
categories:
  - blog
tags:
  - nuke
  - environment variable
---

If you have environment variables set up for paths, you may want to add them as favourites to the file browser favorites. 
This is possible with a tcl command, for example my environment variable `$AN_PROJECT`

```
[getenv AN_PROJECT]
```

Once you'ved added one you can make more by following the syntax in the preference file and adding more, in here:
```
c:\Users\sreeves\.nuke\FileChooser_Favorites.pref
```

by removing any specific dialog types like  `-image` it should show everywhere
```
add_favorite_dir $AN_PROJECT $AN_PROJECT "" "[getenv AN_PROJECT]/"
add_favorite_dir $ANALOG_PROJECTS $ANALOG_PROJECTS "" "[getenv ANALOG_PROJECTS]/"
```

![image](https://user-images.githubusercontent.com/12150445/144646920-63e3586a-0fda-48ef-8679-ace26f2ce2f4.png)
