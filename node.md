Node
============
I denne guide vil jeg fortælle hvordan opsætter en NodeJS application med Express, PM2 og NGINX

Først skal du oprette forbindelse til din server med [SSH](#).

NPM(Node Package Manager)
-------------------------

Første gang du kommer ind på din server, har den ikke alle redskaberne til at lave din NodeJS application, derfor skal vi lige installere dem.

Vi starter med NPM:

I din terminal skal du skrive følgende:

`apt install npm`

Hvis den giver dig en error, så prøv at skrive:

`apt install npm --fix-missing`

Hvis det hele er gået efter planen burde det være installeret nu, du kan tjekke med denne kommando:

`npm -v`

Hvis du får et version nummer, er det installeret korrekt.

PM2
---

Det kan være du har flere applicationer, det kan være svært at holde styr på. Derfor installere vi PM2 som er en daemon process manager.

Installationen er lige frem:

`npm install pm2 -g`

Dette installere PM2 globalt.

Hvis du skriver `pm2 -v` får du et version nummer og så er PM2 installeret korrekt.

NGINX
-----

NGINX er et stykke software som kan forbinde eksterne porte til dine interne porte, f. eks. `443 -> 3000`.

Installationen til NGINX er lidt mere kompliceret end de andre ting vi har installeret. Men bare følg denne guide, så burde det gå:)

Først navigere til `/etc/apt/sources.list.d/`:

`cd /etc/apt/sources.list.d/`

Opret herefter filen `nginx.list`:

`touch nginx.list`

hvis du nu skriver `ls`, så burde du se filen ovenover.

Gå nu ind i filen med `nano`.

`nano nginx.list`

Sæt nu det her ind:

`deb http://nginx.org/packages/ubuntu/ bionic nginx  
deb-src http://nginx.org/packages/ubuntu/ bionic nginx`

[GitHub Repo](https://github.com/MJHC/ruslanServerGuide)