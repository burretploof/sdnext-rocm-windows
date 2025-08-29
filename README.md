# sdnext-rocm-windows
Information on how to install SD.Next with ROCm support on Windows
This is only relevant for AMD users.

## Notes
***Please read these before doing anything!***
  - This guide is only relevant as long as SD.Next does not perform an automatic installation of ROCm pytorch wheels on Windows
  - Check whether your AMD GPU is supported by ROCm in the first place; unsupported GPUs will not work
  - This guide will tell you how to install the current ***pre-release*** versions of ROCm pytorch wheels
    - Stability can vary between builds; if one doesn't work, try another
    - Your driver might time out or your system might crash; while nothing should really "break", remember that ***you're doing this at your own risk!***
    - I cannot offer one-on-one support
  - I give no guarantees that this will work on your system

## Requirements
  - Supported AMD GPU
    - [The supported architectures can be gleaned from this site](https://github.com/ROCm/TheRock/blob/main/RELEASES.md)
    - It also has a table telling you which series has which 'gfx' codename
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
This is assuming you have fulfilled the requirements already; meaning you have a supported GPU, the latest drivers, Git and Python (and optionally miniconda) installed.

  1. Follow [the SD.Next install guide to receive a working installation](https://vladmandic.github.io/sdnext-docs/Installation/)
      - Again, I recommend using Miniconda to set up the desired version of Python in an isolated environment. SD.Next will create its own virtual environment, but it's good to be able to switch versions easily.
      - If I recall correctly, the original backend doesn't matter here, but I still need to confirm this
  2. Terminate the SD.Next process
  3. In your (conda/regular) shell, navigate to the SD.Next folder and activate the venv
      - `venv\Scripts\activate.bat` (for cmd)
      - `.\venv\Scripts\Activate.ps1` (for Powershell, might require script execution privileges)
      - If the shell says (venv) next to your cursor, then it worked correctly
  5. Visit the index URL for your GPU architecture that is listed [on this site](https://github.com/ROCm/TheRock/blob/main/RELEASES.md)
  6. Check the rocm, rocm-sdk-core, rocm-sdk-devel, rocm-sdk-libraries-your-gfx-arch, torch and torchvision folders to find the latest **matching** builds for Windows ("win" in the name) and your version of Python (cp311 = Python 3.11, cp312 = Python 3.12 and so on)
      - Matching means that you need to find packages that have the same build date, otherwise pip will refuse installing them
  7. In your shell, type `python -m pip install ` and then paste each of the links to the packages, separated by spaces and hit enter
    - For example, this could look like this ***(do not use this example!)***:
       - `python -m pip install https://d2awnip2yjpvqn.cloudfront.net/v2/gfx120X-all/rocm-7.0.0rc20250825.tar.gz https://d2awnip2yjpvqn.cloudfront.net/v2/gfx120X-all/rocm_sdk_core-7.0.0rc20250825-py3-none-win_amd64.whl https://d2awnip2yjpvqn.cloudfront.net/v2/gfx120X-all/rocm_sdk_devel-7.0.0rc20250825-py3-none-win_amd64.whl https://d2awnip2yjpvqn.cloudfront.net/v2/gfx120X-all/rocm_sdk_libraries_gfx120x_all-7.0.0rc20250825-py3-none-win_amd64.whl https://d2awnip2yjpvqn.cloudfront.net/v2/gfx120X-all/torch-2.9.0a0%2Brocm7.0.0rc20250825-cp312-cp312-win_amd64.whl https://d2awnip2yjpvqn.cloudfront.net/v2/gfx120X-all/torchvision-0.24.0a0%2Brocm7.0.0rc20250825-cp312-cp312-win_amd64.whl`
       - The reason why I recommend this type of approach is that pip would otherwise download tons of versions in an attempt to find one that is compatible with all other installed packages; *but it likely won't find one at this time.* This approach saves you time and network traffic.
  8. pip should now download, compile and install these packages
  9. Downgrade numpy to a working version by running the following in the venv shell
      - `python -m pip uninstall numpy` followed by
      - `python -m pip install numpy==1.26.4`
  10. Deactivate the venv by running `deactivate` in the shell
  11. Run SD.Next like this:
      - `webui.bat --debug --use-rocm --skip-torch --experimental`
      - `--debug` will enable debug output which helps with diagnosing issues
      - `--use-rocm` probably has no effect at this time but it's nice to have 😉
      - `--skip-torch` will skip checks for torch packages so that they don't get replaced by the regular versions
      - `--experimental` will skip checks for various packages and libraries, allowing you to attempt running SD.Next even if something is deemed incompatible (i.e. prevents numpy from being replaced with a newer version)
      - Keep in mind that your installation of SD.Next might break during this process. If it does, the easiest "fix" is to start over. 😅
  12. It should work? 🤔
