# sdnext-rocm-windows
Information on how to install SD.Next with ROCm support on Windows
This is only relevant for AMD users.

## Notes
***Please read these before doing anything!***
  - This guide is only relevant as long as SD.Next does not perform an automatic installation of ROCm pytorch wheels on Windows
  - Check whether your AMD GPU is supported by ROCm in the first place; unsupported GPUs will not work
  - This guide will tell you how to install the current ***pre-release*** versions of ROCm pytorch wheels
    - Stability can vary between builds; if one doesn't work, try another
    - Your driver might time out or your system might crash, while nothing should really "break", remember that ***you're doing this at your own risk!***
    - I cannot offer one-on-one support
  - I give no guarantees that this will work on your system

## Requirements
  - Supported AMD GPU
    - [The supported architectures can be gleaned from this list](https://github.com/ROCm/TheRock/blob/main/RELEASES.md)
    - To figure out what architecture your GPU is, you can search for your GPU model name online; there are sites that will tell you which "gfx" codename it has
    - ***Also verify, that TheRock is uploading builds for your specific GPU architecture! You can do this by visiting the index URL and checking if packages for torch and torchvision exist***
  - Latest AMD Adrenaline driver
  - Git
  - Python
    - At the time of writing, ROCm pytorch wheels support Python 3.11, 3.12 and 3.13
    - Personally, I've had the most success with Python 3.12
  - OPTIONAL (but recommended): (Mini)conda
    - (Mini)conda allows you to install multiple versions of Python and isolate them entirely
    - This allows you to try different versions of Python without having to uninstall your system-wide Python installation
    - How it is used is beyond the scope of this guide but do refer to the [conda docs](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) to find out how to use it

## Procedure
WIP...
