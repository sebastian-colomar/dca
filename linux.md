```
ss --listening --numeric --tcp
sudo ss --listening --numeric --tcp --processes

sudo apt install --yes nginx
sudo ss --listening --numeric --processes | grep :80\ 
