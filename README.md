# How to setup a Google Cloud Instance for Machine Learning for DFDC
## Cheat sheet for advanced users

### Basics

- Create a GCP account at https://cloud.google.com/ and setup billing details

- Create a project

- Create a VM instance

- Setup external ip of the VM to static, set up firewell based on the refernece article [1]

- Add a persistent drive to VM, see reference [2]

E.g. Device id: sdb, mount dir: dfdc_data

#### With Format
```
sudo lsblk && sudo mkfs.ext4 -m 0 -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/sdb && sudo mkdir -p /mnt/disks/dfdc_data && sudo mount -o discard,defaults /dev/sdb /mnt/disks/dfdc_data && sudo chmod a+w /mnt/disks/dfdc_data
```
#### Without Format
```
sudo mount -o discard,defaults /dev/sdb /mnt/disks/dfdc_data && sudo chmod a+w /mnt/disks/dfdc_data
```

#### Download data
- Use CurlWGet chrome extension to get data URL

Mine looks this:
```
wget --header="Host: storage.googleapis.com" --header="User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36" --header="Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9" --header="Accept-Language: en-US,en;q=0.9,th;q=0.8,zh-CN;q=0.7,zh;q=0.6" --header="Referer: https://www.kaggle.com/" --header="Cookie: cuntwars_user_id=jilW0PIXf0; _ga=GA1.3.2052114344.1570426301" --header="Connection: keep-alive" "https://storage.googleapis.com/kaggle-competitions-detached-data/16880/dfdc_train_all.zip?GoogleAccessId=web-data@kaggle-161607.iam.gserviceaccount.com&Expires=1581023480&Signature=jHCSWvKFnN8A5BmhWl%2FpPr8GglOMwcS1v%2FrQUKw9u9UlrajS2t%2B25kG4l6A81bQtyFBniKfcrxS6sxV18q3uPEDezffMWacAbbHbLUo%2BSRNMTV8%2BIpXhPHn7j2CESfojchgDOMcJ%2FrqKoQ7kY8tGAb0l35w7vYMXXmw3kXyM4do1D6anpnMhz2k1IXPM3HrlrxZOfCeQ3slsgOsAJWfqshnDr01Ex%2FXgO2sWk82HwmU%2F%2BUhg%2FmaJgMSFsnd1mRM37TVZx34pegM3Y%2BnmpP1Lmzlh3blRCm7MpsV9LEEsnXobuP3YwZNEzzF1nh7C%2BG7jtJyuz5JHkJ6pyoHKtHunTw%3D%3D" -O "dfdc_train_all.zip" -c
```
- Change "dfdc_train_all.zip" to "[YOUR_PERSISTENT_DISK_PATH]/dfdc_train_all.zip"

- Unzip data to your disk
```
unzip dfdc_train_all.zip -d /mnt/disks/dfdc_data
```

- Wait, what? More .zips inside LOL. I am extracting them to a folder called 'data'
```
unzip dfdc_train_part_00.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_01.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_02.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_03.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_04.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_05.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_06.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_07.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_08.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_09.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_10.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_11.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_12.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_13.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_14.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_15.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_16.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_17.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_18.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_19.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_20.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_21.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_22.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_23.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_24.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_25.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_26.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_27.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_28.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_29.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_30.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_31.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_32.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_33.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_34.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_35.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_36.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_37.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_38.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_39.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_40.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_41.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_42.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_43.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_44.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_45.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_46.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_47.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_48.zip -d /mnt/disks/dfdc_data/data/ && unzip dfdc_train_part_49.zip -d /mnt/disks/dfdc_data/data/

```
I'm lazy, make a for loop if you wish.

### Setup Jupyter

```
# Download Anaconda
$ wget http://repo.continuum.io/archive/Anaconda3-4.0.0-Linux-x86_64.sh

# License, path, etc. (Don't forget to say 'yes' to prepending path)
$ bash Anaconda3-4.0.0-Linux-x86_64.sh

# Activate conda
$ source .bashrc

# Quick check
$ conda --version 

# Run pip to test
pip install torch
pip install torchvision
```
Running Jupyter in browser via external IP.
```
$ jupyter notebook --ip=0.0.0.0 --port=8080 --no-browser &
```
Available at
```
http://[EXTERNAL_IP]:8080
```

### Using buckets
- Create a bucket

- Getting permission
```
gsutil config -b
```

- Copy some data to bucket
```
gsutil -m cp -r /mnt/disks/dfdc_data/data gs://[BUCKET_NAME]
```

- Or copy data from bucket
```
gsutil -m cp -r gs://[BUCKET_NAME]/mnt/disks/dfdc_data/data
```



## Reference
- [1] https://towardsdatascience.com/running-jupyter-notebook-in-google-cloud-platform-in-15-min-61e16da34d52
- [2] https://cloud.google.com/compute/docs/disks/add-persistent-disk?hl=en_US&_ga=2.190677090.-876535034.1572141372&_gac=1.82478436.1580475048.CjwKCAiA98TxBRBtEiwAVRLqu19jsp_c8qQn3MXIOej2p37h8OknzWikS9e8h5-RjtZsMABdHadmTxoCHb4QAvD_BwE#formatting
