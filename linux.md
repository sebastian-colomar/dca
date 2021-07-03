```
ss --listening --numeric --tcp
sudo ss --listening --numeric --tcp --processes

sudo apt install --yes nginx
sudo ss --listening --numeric --processes | grep :80\ 

sudo apt purge nginx --yes
sudo apt autoremove --yes
