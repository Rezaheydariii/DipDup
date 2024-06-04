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

Now, For educational purposes, we'll create a project from scratch
Well, at this stage, we're creating a raw project. Let's move forward to understand what's going on.
```
dipdup new
```
* so choose [none]
#Choose a project template:
demo_blank  Empty config for a fresh start
next:
#Enter project name (the name will be used for folder name and package name)

#Choose PostgreSQL version. Try TimescaleDB when working with time series ...
![all](https://github.com/Rezaheydariii/DipDup/assets/140112620/3f84c2c8-90ff-4d65-84cf-42c6570c5d2a)

خب! از اینجا به بعد ترجیح می د که فارسی جلو بریم. حالاکه یک پروژه با نام مدنظرتون و کاملا خام درست کردید قراره با یکسری 
دیتا تمرین کنیم...
اول از همه وارد دایرکتوری پروژه بشید. project name یعنی اسم پروزه خودتون
```
cd project name
```
حالا برا اینکه بدونید جه محتوایی توی این دایرکتوری دارید یکبار ls رو اجرا کنید.
در این مرحله قراره محتوای فایل dipdup.yaml رو با مقادیر تمرینی تغییر بدیم.
```
nano dipdup.yaml
```
کد زیر رو جایگزین محتوای قبلی کنید.
```
spec_version: 2.0
package: kakarot

datasources:
  mainnet_node:
    kind: evm.node
    url: https://sepolia-rpc.kakarot.org

contracts:
  some_contract:
    kind: evm
    address: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
    typename: not_typed

indexes:
  evm_index:
    kind: evm.subsquid.transactions
    datasource: mainnet_node
    handlers:
      - callback: on_output_transaction
        from: some_contract
    last_level: 4631


database:
  kind: sqlite
  path: data/kakarot.sqlite
```
C + X >> Y >> inter
حالا باید دایرکتوری رو بروز کنیم:

```
dipdup init
```

حالا دوباره در دایرکتوری کامند ls رو اجرا کنید... فولدری به نام models دارید. با دستور نانو فایل داخل رو باز کنید.... 
```
nano models/__init__.py
```

کدهای زیر رو جایگزین کدهای داخل فایل  کنید:

```
from dipdup import fields
from dipdup.models import Model

class Transaction(Model):
    hash = fields.TextField(pk=True)
    block_number = fields.IntField()
    from_ = fields.TextField()
    to = fields.TextField(null=True)

    created_at = fields.DatetimeField(auto_now_add=True)
    ```
دوباره به دایرکتوری پروژه خودتون برگردید و فایل هندلر رو باز کنید.

```
nano handlers/on_output_transaction.py
```

کد زیر رو جایگزین کدهای داخل فایل کنید...
حتمن یادتون باشه که نام پروژه شما چیه "kakarot" این نام به صورت پیشفرض قرار داده شده و باید اسم پروژه تون رو اینجا قرار بدید.

```
from dipdup.context import HandlerContext
from dipdup.models.evm_node import EvmNodeTransactionData
from dipdup.models.evm_subsquid import SubsquidTransactionData

from kakarot import models as models

async def on_output_transaction(
    ctx: HandlerContext,
    transaction: SubsquidTransactionData | EvmNodeTransactionData,
) -> None:
    await models.Transaction(
        hash=transaction.hash,
        block_number=transaction.block_number,
        from_=transaction.from_,
        to=transaction.to,
    ).save()
```

حلا میتونید پروژه رو ران کنید با کامند...
```
dipdup run
```

خروجی بدین شکل خواهدبود:

![Capture](https://github.com/Rezaheydariii/DipDup/assets/140112620/60d77c03-ad52-40fe-83be-de36111b2187)










