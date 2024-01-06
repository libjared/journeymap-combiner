# journeymap-combiner
how to update journeymap data by merging in newer data on top.

1. ok you pretty much just copy the newer journeymap/data/mp/SERVERNAME overtop your older journeymap dir
1. if there's any dupes you tell explorer (KDE dolphin in my case) to auto-rename (like "blah (1).png")
1. delete `cache` dirs like this (I actually dont know what these files do, but "cache" sounds replacable tbh)
   ```bash
   cd journeymap/data/mp/SERVERNAME
   find -type d -name "cache" -exec rm -rv "{}" \+
   ```
1. combine those duplicate sections of the map via imagemagick composition
   ```bash
   find -type f -regex ".*([0-9]+).png" -exec echo "{}" \; | while read newf; do
     oldf="${newf% \(1\).png}.png"
     magick composite "$newf" "$oldf" "$oldf"
     rm "$newf"
   done
   ```
