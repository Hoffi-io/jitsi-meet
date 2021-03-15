# Hoffi - Jitsi Meet

**Remarque :** ~ ou $HOME, représente ici _/home/abl_

## Mise à jour

Prendre la version récente d'Hoffi Jitsi Meet

```
git pull -X theirs origin master
```

Sachant qu'on avait déjà lancé `npm update && npm install` et qu'Nginx pointe déjà vers _/home/abl/jitsi-meet_

```
cd ~/jitsi-meet/
npm install lib-jitsi-meet --force && make
```

## Installation

A faire seulement une fois

- Configurer son compte git en remplaçant _&lt;username&gt;_ et _&lt;email&gt;_ suivantes par les valeurs appropriées

```
sudo git config --global user.name <username>
sudo git config --global user.email <email>
```

- Cloner Jitsi-Meet pour l'utilisateur en cours. Ici : _abl_

```
sudo chown abl:abl -R /home/abl
git clone https://github.com/Hoffi-io/jitsi-meet-1 ~/jitsi-meet
```

- Installer une version récente de Node

```
touch ~/.bashrc
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
nvm install v14.15.4
```

- Faire un build de Jitsi Meet

```
cd ~/jitsi-meet/
npm update && npm install
make all
```

- Faire un build de Lib Jitsi Meet

```
cd ~
git clone https://github.com/jitsi/lib-jitsi-meet.git

sed -i 's&"lib-jitsi-meet": "github:jitsi/lib-jitsi-meet.*",&"lib-jitsi-meet": "file:../lib-jitsi-meet",&' ~/jitsi-meet/package.json

cd ~/lib-jitsi-meet
npm update

rm -rf ~/jitsi-meet/node_modules/lib-jitsi-meet && cd ~/jitsi-meet/ && npm install lib-jitsi-meet --force && make
```

- Mettre à jour Nginx

```
sudo sed -i 's&/usr/share/jitsi-meet&/home/abl/jitsi-meet&' /etc/nginx/sites-available/meet-dev.hoffi.io.conf

sudo service nginx restart
```
