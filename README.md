# KiCad Round Tracks
A subdivision- and/or native arc-based track rounding plugin for KiCad.

The goal is to algorithmically melt a PCB design, smoothing all tracks in a predictable manner. 

![example](https://mitxela.com/img/uploads/sw/kicad/example.png)

For best results, use in conjunction with this [teardrop plugin](https://github.com/NilujePerchut/kicad_scripts).

I have written extensively about my kicad melting experiments [here](https://mitxela.com/projects/melting_kicad) and [here](https://mitxela.com/projects/melting_kicad_2).

## Use

__For the KiCad 5 compatible version, please use the kicad-5 branch.__

The intended use is to have a source PCB file, which is easy to edit, and a rendered output file, generated by this plugin. After running DRC on the output, you can then go back and adjust the original file before running the plugin again.

If you select part of the board, only the selected tracks will get melted. If nothing is selected, the whole file will be processed.

"Create a new file" does not immediately write anything to disk, it merely appends "-rounded" to the filename so that the next time you hit Save it won't overwrite the original.

If "use native" is ticked, the rounded sections are implemented using KiCad 6's native PCB arcs. These _can_ be adjusted after the script has run, but I still recommend keeping the unrounded source PCB file. I have tried hard to ensure that the output PCB is the same, regardless of if native or subdivision-based arcs are used. If "use native" is ticked, the number-of-passes parameter is ignored.

Otherwise, the curves are generated from many small straight sections. 2 or 3 passes is usually sufficient. There is no loading bar, so if the design is complicated the interface might freeze for a few seconds while it runs. You can set the number of passes to be 1, and run the plugin repeatedly, and it will give the same results, while letting you inspect the intermediate stages.

"Radius" is the maximum radius curve that would result from a 90° bend. A smaller curve will be used if the tracks are shorter, or if the angle between them is sharper. The resulting curves will always pass through at least one point of the original tracks, so individual curves can be controlled by splitting tracks into smaller sections. If a curve has too large a radius, placing a small 45° bend will make it smaller. Similarly, if a bigger radius is needed for certain tracks, you can draw an approximate curve with free-angle tracks to achieve this.

## Installation
Now available from the Plugin and Content manager.

For manual install, clone or unzip this repository in a KiCad plugin folder. You can list the exact paths where KiCad will search for plugins by opening the scripting console in pcbnew and running:
```
import pcbnew
print(pcbnew.GetWizardsSearchPaths())
```

Under Preferences / Preferences / PCB Editor / Action Plugins, you can choose whether to have a button on the toolbar for quick access to the plugin.

## History
This plugin is based on [flexRoundingSuite](https://github.com/jcloiacon/flexRoundingSuite) by Julian Loiacono and [kicad-round-tracks](https://github.com/stimulu/kicad-round-tracks) by Antoine Pintout. My contribution updated the algorithm so that subdivisions are applied equally, resulting in smoother tracks with fewer clearance errors.

Original copyright Miles McCoo, 2017  
Extensively modified by Julian Loiacono, Oct 2018  
Multi-layer support and repacked as action plugin by Antoine Pintout, May 2019  
Updated subdivision algorithm by mitxela, Jan 2021  
