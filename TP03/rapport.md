Voici la liste des commandes effectuées par l'attaquant : 
:

    id : Cette commande affiche l'ID de l'utilisateur et l'ID de groupe de l'utilisateur courant.
    cat /etc/passwd : Cette commande affiche le contenu du fichier /etc/passwd, qui contient des informations sur les comptes d'utilisateurs du système.
    cat /etc/hosts : Cette commande affiche le contenu du fichier /etc/hosts, qui contient des correspondances entre adresses IP et noms d'hôtes.
    ls /var/www/html : Cette commande liste les fichiers et les répertoires dans le répertoire /var/www/html, qui est un emplacement courant pour le contenu web.
    pwd : Cette commande affiche le répertoire de travail courant.
    cd /home/b0sch/ : Cette commande change le répertoire de travail courant en /home/b0sch/.
    ping 138.66.89.12 : Cette commande envoie une requête de ping à l'adresse IP 138.66.89.12, qui peut être utilisée pour tester la connectivité réseau ou découvrir des hôtes sur le réseau.
    cat /etc/shadow : Cette commande affiche le contenu du fichier /etc/shadow, qui contient des hachages de mots de passe cryptés pour les comptes d'utilisateurs du système.
    l : Ceci n'est pas une commande valide, donc cela a probablement entraîné un message d'erreur.
    ls -lah : Cette commande liste les fichiers et les répertoires dans le répertoire courant en format long, y compris les fichiers cachés.
    crontab -e : Cette commande ouvre le fichier de crontab de l'utilisateur dans l'éditeur par défaut, permettant à l'utilisateur de planifier des commandes à exécuter à des moments spécifiques.
    zip -r --password $(cat /tmp/mypassword) bosch_cyber_tools.zip /home/b0sch/bosch_cyber_tools : Cette commande crée une archive ZIP protégée par mot de passe du répertoire /home/b0sch/bosch_cyber_tools et la stocke dans le répertoire courant. Le mot de passe pour l'archive est lu à partir du fichier /tmp/mypassword.
    mkdir /opt/leak : Cette commande crée un nouveau répertoire appelé /opt/leak.
    mv bosch_cyber_tools.zip /opt/leak : Cette commande déplace le fichier bosch_cyber_tools.zip dans le répertoire /opt/leak.
    rm /tmp/mypassword : Cette commande supprime le fichier /tmp/mypassword, qui contenait le mot de passe pour l'archive ZIP.
    cd .. : Cette commande change le répertoire de travail courant au répertoire parent.
    rm -rf /home/b0sch/bosch_cyber_tools : Cette commande supprime de manière récursive le répertoire /home/b0sch/bosch_cyber_tools et son contenu, ce qui peut entraîner la suppression de données ou d'outils importants.
    
   On récupère le zip chiffré en local : docker cp 2676834c4aa6:/opt/leak/bosch_cyber_tools.zip /home/kali/Documents
   
   On installe zip2john et john the ripper :
   
  git clone https://github.com/openwall/john -b bleeding-jumbo john
  cd john/src/
  ./configure
  make -s clean && make -sj4
  
  On execute zip2john :
  cd ../run/
  ./zip2john ../../Documents/bosch_cyber_tools.zip > bosch_cyber_tools.hash
  
 Puis john the ripper pour tenter de bruteforcer le mot de passe de l'archive :
 john bosch_cyber_tools.hash

  
  
