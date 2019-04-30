# Exercicis resolts

1. Crear un entorn a partir d'un box que disposeu, per exemple bionic64. On definirem una xarxa privada, assignant la IP 192.168.168.100. Un cop aixecat l'entorn, connectar-se via ssh i instal·lar apache2. Comprovar com des del navegador des del vostre ordinador si poseu la IP anterior apareix la pàgina per defecte d'Apache. [Solució](solucions/exercici1.md)

1. Definir un entorn a partir del box de bionic64 que tingueu, el qual aprovisionarem amb un script que anomenarem *bootstrap.sh* i que haurà de fer de fer la instal·lació de nginx,  i fer un enllaç simbòlic entre la carpeta *web* on tindreu un arxiu *index.html* amb la carpeta */var/www*. [Solució](solucions/exercici2.md)

1. Crear un entorn amb dos màquines Apache2, el contigut html de cada pàgina es trobarà a dues carpetes web1 i web2. Totes dues màquines usaran xarxa privada (192.168.100.100 i 192.168.100.102), s'aprovisionaran mitjançant un script que pot basar-se al de l'exercici anterior. Cal que el index.html de la primera màquina tingui un hipervincle per accedir a la pàgina web del segon servidor. Per poder fer servir el mateix script en totes dues màquines, mapejarem la carpeta corresponent al contingut web (web1 i web2) a /srv/web.
[Solució](solucions/exercici3.md)

[<< Tornar a índex AA3](../README.md)
