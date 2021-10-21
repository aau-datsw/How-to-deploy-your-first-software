Node
============
I denne guide vil jeg fortælle hvordan opsætter en NodeJS application med Express, PM2 og NGINX.

Hvis du har problemer med at installere eller redigere filer i din server, kan du prøve at bruge `sudo` foran din kommando f. eks. `sudo nano test.txt`.

Først skal du oprette forbindelse til din server med [SSH](#).

NPM(Node Package Manager)
-------------------------

Første gang du kommer ind på din server, har den ikke alle redskaberne til at lave din NodeJS application, derfor skal vi lige installere dem.

Vi starter med NPM:

I din terminal skal du skrive følgende:

```bash
apt install npm
(undgå at kopiere $)

Hvis den giver dig en error, så prøv at skrive:

```
$ apt install npm --fix-missing
```

Hvis det hele er gået efter planen burde det være installeret nu, du kan tjekke med denne kommando:

```
$ npm -v
```

Hvis du får et version nummer, er det installeret korrekt.

PM2
---

Det kan være du har flere applicationer, det kan være svært at holde styr på. Derfor installere vi PM2 som er en daemon process manager. Det betyder at du kan køre flere NodeJS servere i baggrunden. PM2 er primært til at kører node i production, da det også giver et dashboard med status på din server, samt mails hvis din server går ned. Et eksempel er fx 
![image](https://user-images.githubusercontent.com/15074677/138341402-6f5d3da7-2519-453b-b3f8-5564815fc63a.png)


Installationen er lige frem:

```
$ npm install pm2 -g
```

Dette installere PM2 globalt.

Hvis du skriver `pm2 -v` og får du et version nummer og så er PM2 installeret korrekt.

NGINX
-----

NGINX er et stykke software som kan forbinde eksterne porte til dine interne porte, f. eks. `443 -> 3000`.

Installationen til NGINX er lidt mere kompliceret end de andre ting vi har installeret. Men bare følg denne guide, så burde det gå:)

Først navigere til `/etc/apt/sources.list.d/`:

```
$ cd /etc/apt/sources.list.d/
```

Opret herefter filen `nginx.list`:

```
$ touch nginx.list
```

hvis du nu skriver `ls`, så burde du se filen ovenover.

Gå nu ind i filen med `nano`.

```
$ nano nginx.list
```

Sæt nu det her ind og tryk på CRTL + X derefter Y og bagefter ENTER:

```
deb http://nginx.org/packages/ubuntu/ bionic nginx  
deb-src http://nginx.org/packages/ubuntu/ bionic nginx
```



Herefter skal vi tilføje NGINXs repo siging key og installere det:

```
$ wget --quiet http://nginx.org/keys/nginx_signing.key && sudo apt-key add nginx_signing.key
$ sudo apt install nginx
```

Tjek nu om det er installeret korrekt:

```
$ nginx -v
```

Hvis du får et version nummer, så har er NGINX blevet installeret korrekt. Nu skal du gøre sådan at det starter hver gang system starter. Det gør du sådan her:

```
$ systemctl status nginx
$ systemctl enable nginx
$ systemctl status nginx
```

Det vil også være en god idé at opsætte en firewall, det gør vi med UFW(Uncomplicated Firewall):

```
$ ufw allow 22/tcp
$ ufw allow 80/tcp
$ ufw allow 3000/tcp
$ ufw enable
```
22: SSH port, 80: HTTP port, 3000: NodeJS port.

Tjek om UFW er blevet aktiveret, med nedstående kommando:

```
$ ufw status
```

Nu til det sjove. Vi skal have konfigureret nginx til at reverse proxy til din server, det gør sådan her:

```
$ nano /etc/nginx/conf.d/sysmon.conf 
```

Sæt nu det her ind og gem filen:

```
server {
    listen 80;
    server_name DIN_URL;

    location / {
        proxy_set_header   X-Forwarded-For $remote_addr;
        proxy_set_header   Host $http_host;
        proxy_pass         http://DIN_IP:3000;
    }
}
```

Nu skal vi bare genstarte NGINX:

```
$ systemctl restart nginx
```

Nu har du sat NGINX op til at reverse proxy til din server:)

NodeJS
------

Nu til det mest spændende, programmering af serveren.

start med at gå til din home folder ved at skrive `cd` i din terminal.

herefter lav en mappe kaldet website:

```
$ mkdir website
```

naviger nu til mappen:

```
$ cd website
```

Herefter skal vi initialisere et node projekt:

```
$ npm init -y
```

Hvis du nu skriver `ls` vil der være en `package.json` fil, det er her alle dine dependencies og scripts til at køre programmet ligger.

Lav nu en fil kaldet `server.js`:

```
$ touch server.js
```

Ændre nu `"main":"index.js"` i `package.json` til `"main":"server.js"`. og når vi nu er i gang, skal du også lige ændre:

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
},
```
-->
```json
"scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
},
```

Herefter skal vi hente nogle dependencies til din server, skriv følgende i din terminal:

```
npm i -D nodemon
npm i express
```

Nu har vi hentet Express og Nodemon til din server. Nodemon er en development dependency, derfor har vi skrevet `-D`. Nodemon restarter selv din server, hvis du laver ændringer i dine projekt filer. Prøv skriv `cat package.json` i din terminal og du burde se at Nodemon lægger under `devDependencies`. 

Gå nu ind i din `server.js` fil med `nano` og skriv følgende:

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => console.log(`My first NodeJS app runnig on: http://localhost:${port}`));
```

Når du har gemt filen kan du nu starte din app med PM2:

```
$ pm2 start server.js
```

Nu burde du se en tabel med din server i. Prøv nu at skrive dit domæne ind i din browser, du skulle meget gerne se teksten "Hello World!".

Hvis du gerne vil skrive en HTML fil og tilføje noget CSS og JavaScript, så kan jeg anbefale at tage et kig på mit [GitHub Repo](https://github.com/MJHC/ruslanServerGuide).

Her er et mit eksempel på en hjemmeside jeg har lavet i mens jeg skrev denne guide [mjhcsoftware.engineer](http://mjhcsoftware.engineer/).

Hvis du vil servere flere HTML dokumenter end en index.html, kan jeg anbefale at bruge [EJS](https://www.digitalocean.com/community/tutorials/how-to-use-ejs-to-template-your-node-application), da express ikke helt kan finde ud af at servere flere end en HTML fil, uden at bruge en view engine. Normalt er express brugt til at lave backends, hvorimod hvis man gerne vil lave hjemmesider, bruger man et framework som `Angular`, `react` eller `vue`.

Hvis du har problemer med at få din server op at køre, kan du bare pinge mig på Ruslan 2021 Discord Serveren: @DISPLAY#8252

