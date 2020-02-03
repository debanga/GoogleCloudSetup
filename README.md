# How to setup a Google Cloud Instance for Machine Learning in 5 minutes
## Cheat sheet for advanced users

- Create a GCP account at https://cloud.google.com/ and setup billing details

- Create a project

- Create a VM instance

- Setup external ip of the VM to static, set up firewell based on the refernece article [1]

- Add a persistent drive to VM, see reference [2]

E.g. Device id: sdb, mount dir: dfdc_data

### With Format
```
sudo lsblk && sudo mkfs.ext4 -m 0 -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/sdb && sudo mkdir -p /mnt/disks/dfdc_data && sudo mount -o discard,defaults /dev/sdb /mnt/disks/dfdc_data && sudo chmod a+w /mnt/disks/dfdc_data
```
### Without Format
```
sudo mount -o discard,defaults /dev/sdb /mnt/disks/dfdc_data && sudo chmod a+w /mnt/disks/dfdc_data
```

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
