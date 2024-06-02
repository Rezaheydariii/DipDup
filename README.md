# DipDup
![Capture](https://github.com/Rezaheydariii/DipDup/assets/140112620/d270fab3-f8b0-4d31-b2f1-88f2a356fdc1)


DipDup is a Python framework for building smart contract indexers that helps developers focus on business logic. DipDup-based indexers are selective, allowing for faster indexing times and decreased API load. Follow the steps on this page to create your first DipDup indexer for output transactions from a specific address in just a few minutes. This involves setting up the indexing environment, configuring the indexer, and storing the results in a database.
# To use DipDup:
keep in mind that you need to have an upgraded Linux server with Ubuntu 24 and Python 3.11.
Now, we can easily get started with DipDup together:
#  install Python 3.11 :
```
sudo apt update
```
# Install Required Dependencies:
```
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
```
# Download and Extract Python 3.11:
```
wget https://www.python.org/ftp/python/3.11.0/Python-3.11.0.tgz
tar -xf Python-3.11.0.tgz
```
```
cd Python-3.11.0
./configure --enable-optimizations
make -j$(nproc)
sudo make altinstall
```
* This part may take more time.

# check V. :
```
python3.11 --version
```
Now,  Create a Virtual Environment using venv
# Create a Virtual Environment:
```
python3.11 -m venv myenv
```
#  Activate the Virtual Environment:
```
source myenv/bin/activate
```
#  Install pipx and DipDup in the Virtual Environment:
```
pip install pipx
pipx install dipdup
```
```
pip uninstall dipdup
pip install dipdup
```
# Now, let's go back to DipDup and install:
```
curl -Lsf https://dipdup.io/install.py | python3
```
![Capture](https://github.com/Rezaheydariii/DipDup/assets/140112620/25355306-3c8e-4b6e-91fe-a92fb6553704)

If you are unable to run DipDup, please check your Virtual Environment again:
```
source myenv/bin/activate
pip show dipdup
```
If it's installed, you should see details about the package.
```
echo $PATH
```
Reinstall DipDup
```
pip uninstall dipdup
pip install dipdup
```
Alternative: Use pipx:
```
pip install pipx
pipx install dipdup
dipdup new
```
Now, For educational purposes, we'll create a project from scratch



