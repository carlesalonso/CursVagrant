# Aprovisionadors

Ja hem vist anteriorment com aprovisionar un entorn Vagrant, en aquest cas Linux, utilitzant comandes de bash inline o utilitzant scripts, però és habitual utilitzar eines de gestió per fer aquestes tasques, sobre tot en entorns més complexos.

Existeixen diverses eines d'aquest tipus que no només serveixen per Vagrant si no que es poden fer servir per altres entorns. L'avantatge d'usar aquestes eines és que són multiplataforma i modulars, de manera que els esquemes d'aprovisionament són altament reutilitzables. Les més populars són:

* Ansible
* Chef
* Salt
* Puppet

Algunes d'aquestes eines com Chef o Puppet requereixen que la màquina client tingui un agent instal·lat, per això, habitualment cal utilitzar boxes que ja vinguin configurats per l'aprovisionador específic. Altres com Ansible (potser la més popular avui dia) no necesita aquests agents, ja que configura la instància via ssh.

La major part d'aquestes eines estan pensades per treballar des d'ordinadors Linux o Mac, per sort, ara amb Windows 10 podem utilitzar WSL per poder treballar amb les mateixes condicions.

Cal destacar que no són eines específiques per Vagrant, són eines per automatitzar els desplegament de sistemes, permetent utilitzar codi per implementar infraestructures complexes, tant en servidors on-premise com al cloud (AWS,Azure o Google Cloud).