[![License: GPL v3.0](https://img.shields.io/badge/license-GPL%20v3.0-green.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Doors: 9.5](https://img.shields.io/badge/Doors-9.5-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

# DOORS-DXL-Scripts

A collection of useful DOORS DXL Scripts independent from the project.
![User Tab](images/User_tab.png)

## How to install

- Download all files inside of this repo.
- Copy all files into your Doors installation folder ```C:\Program Files (x86)\IBM\Rational\DOORS\9.5\lib\dxl\addins\user```

## Scripts

### Copy Module Path (Ctrl + 2)

Its copies the full path of the current module into the clipboard.

### Copy Object ID (Ctrl + 3)

It copies Id of the selected object(s) into the clipboard. If multiple objects are selected they are will be separated by a newline.

### Copy Object ID (Ctrl + 3)

This script shows baseline comparison windows similar to DOORS's windows with an extra button for Beyond Compare. It creates a .txt export of selected baselines in the background and then compares them with Beyond Compare. It's easier to see the difference with this method.  
 
**TODO:** Add a button for WinMerge too.

![Baseline Comparison Tool](images/BaselineComparisonGUI.png)

### Filter Magic Numbers

All numbers (except 0 and 1) not located inside of square brackets ```[]``` assumed magic number. This script filters them. `

### Copy Links

Its copies links on the source object to the destination object.

### Export all modules into Html

These scripts recursively export all items under the project to a folder as a fancy-styled useful HTML report.

## License

This project is licensed under the terms of the  [GNU General Public License v3.0](https://choosealicense.com/licenses/gpl-3.0/)

Copyright © 2020 M. Serdar Karaman
