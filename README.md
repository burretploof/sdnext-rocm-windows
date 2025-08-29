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
  5. Run the following to install the ROCm libraries as well as torch and torchvision in the venv (this might take a while as the download is large and the ROCm libs will be compiled on your system)
      - `python -m pip install --no-deps --index-url https://d2awnip2yjpvqn.cloudfront.net/v2/gfx120X-all/ rocm[libraries,devel] torch torchvision`
      - With `--no-deps` pip will not try to look for compatible versions and install the latest packages instead. While ROCm 7.0 PyTorch wheels are in pre-release, this is likely desirable.
      - There will likely be warnings from pip telling you about dependencies that don't match. Most of these (except numpy) can *usually* be ignored.
      - To install a specific build, you can visit the sub-directories in the index URL (e.g. [https://d2awnip2yjpvqn.cloudfront.net/v2/gfx120X-all/torch](https://d2awnip2yjpvqn.cloudfront.net/v2/gfx120X-all/torch) ), copy the link of the package you desire and install it using
         - `python -m pip install url-that-you-copy-pasted-here`
         - Please note that the rocm-libraries and rocm-devel packages likely need to match the build date of the torch and torchvision packages
         - Make sure that you pick the correct architecture ("win" in the filename) and the correct python version (cp311 = Python 3.11, cp312 = Python 3.12...)
  6. Downgrade numpy to a working version by running the following in the venv shell
      - `python -m pip uninstall numpy` followed by
      - `python -m pip install numpy==1.26.4`
  7. Deactivate the venv by running `deactivate` in the shell
  8. Run SD.Next like this:
      - `webui.bat --debug --use-rocm --skip-torch --experimental`
      - `--debug` will enable debug output which helps with diagnosing issues
      - `--use-rocm` probably has no effect at this time but it's nice to have 😉
      - `--skip-torch` will skip checks for torch packages so that they don't get replaced by the regular versions
      - `--experimental` will skip checks for various packages and libraries, allowing you to attempt running SD.Next even if something is deemed incompatible (i.e. prevents numpy from being replaced with a newer version)
      - Keep in mind that your installation of SD.Next might break during this process. If it does, the easiest "fix" is to start over. 😅
  10. It should work? 🤔
