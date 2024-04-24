
# server configuration ispconfig

konfigurasi tambahan setelah install ispconfig

# menambahkan swap di ubuntu 22.04

cek apakah swap sudah di konfigurasi atau belum

```
sudo swapon --show
```
jika tidak ada output apapun maka belun ada swap pada sistem ubuntu 22.04

cek apakah disk tersedia pada hardisk
```
df -h
```

membuat file swap (1G size yang digunakan untuk swap) 
```
sudo fallocate -l 1G /swapfile
```

verifikasi size yang digunakan untuk swap
```
ls -lh /swapfile
```
mengaktifkan swap file
```
sudo chmod 600 /swapfile
```
verifikasi permision pada swap file
```
ls -lh /swapfile
```
tandai swap file sebagai swap
```
sudo mkswap /swapfile
```
mengaktifkan swap
```
sudo swapon /swapfile
```
verifikasi swap file sudah aktif

```
sudo swapon --show
```

## membuat swap menjadi permanen
melakukan backup pada fstab

```
sudo cp /etc/fstab /etc/fstab.bak
```

menambahkan informasi swap pada fstab
```
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

## meningkatkan peformance pada swap

menambahkan parameter swappines

```
sudo sysctl vm.swappiness=10
```
menyesuaikan konfigurasi cache pressure
```
sudo sysctl vm.vfs_cache_pressure=50
```
membuat menjadi permanen

```
sudo nano /etc/sysctl.conf
```
tambahkan pada baris akhir
```
vm.swappiness=10
vm.vfs_cache_pressure = 50
```

restart server
```
sudo systemctl reboot
```
# menambahkan timeout koneksi server pada ssh

di server edit pada konfigurasi /etc/ssh/sshd_config

```
sudo nano /etc/ssh/sshd_config
```
cari parameter ClientAliveInterval ClientAliveCountMax
```
#ClientAliveInterval 
#ClientAliveCountMax
```
isi parameter sesuaikan dengan kebutuhan (timeout 1200 * 3 = 3600 detik) 1 jam
```
ClientAliveInterval  1200
ClientAliveCountMax 3
```
restart server
```
sudo systemctl reboot
```