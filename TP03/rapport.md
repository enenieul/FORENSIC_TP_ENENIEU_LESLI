INTRODUCTION
Le site bosch-cyber a été compromis, et que l'attaquant aurait exfiltré des outils secrets très dangereux. Heureusement l'administration a mis le site sous maintenance. nous devons éffectuer une analyse forensique afin de découvrir ce que l'attaquant a exfiltré. 

METHODOLOGIE
Pour faire une analyse forensique des données qui ont été exfiltrées sur le site bosch-cyber, nous allons tout d'abord faire une description des méthodes que nous allons utiliser pour analyser les données et comment nous allons nous en servir.

Après s'être connecté sur le serveur qui a été attaqué par le hacker, nous avons saisi la commande bash history et nous avons pu constater que l'attaquant a éffectué les actions suivantes:

Voici la liste des commandes effectuées par l'attaquant : 

   id : Cette commande affiche l'ID de l'utilisateur et l'ID de groupe de l'utilisateur courant.
   
    cat /etc/passwd : Cette commande affiche le contenu du fichier /etc/passwd, qui contient des informations sur les comptes d'utilisateurs du système.
    
    cat /etc/hosts : Cette commande affiche le contenu du fichier /etc/hosts, qui contient des correspondances entre adresses IP et noms d'hôtes.
    
    ls /var/www/html : Cette commande liste les fichiers et les répertoires dans le répertoire /var/www/html, qui est un emplacement courant pour le contenu web.
    
    pwd : Cette commande affiche le répertoire de travail courant.
    
    cd /home/b0sch/ : Cette commande change le répertoire de travail courant en /home/b0sch/.
    
    ping 138.66.89.12 : Cette commande envoie une requête de ping à l'adresse IP 138.66.89.12, qui peut être utilisée pour tester la connectivité réseau ou découvrir des hôtes sur le réseau.
    
    cat /etc/shadow : Cette commande affiche le contenu du fichier /etc/shadow, qui contient des hachages de mots de passe cryptés pour les comptes d'utilisateurs du système
    
    ls -lah : Cette commande liste les fichiers et les répertoires dans le répertoire courant en format long, y compris les fichiers cachés.
    
    crontab -e : Cette commande ouvre le fichier de crontab de l'utilisateur dans l'éditeur par défaut, permettant à l'utilisateur de planifier des commandes à exécuter à des moments spécifiques.
    
    zip -r --password $(cat /tmp/mypassword) bosch_cyber_tools.zip /home/b0sch/bosch_cyber_tools : Cette commande crée une archive ZIP protégée par mot de passe du répertoire /home/b0sch/bosch_cyber_tools et la stocke dans le répertoire courant. Le mot de passe pour l'archive est lu à partir du fichier /tmp/mypassword.
    
    mkdir /opt/leak : Cette commande crée un nouveau répertoire appelé /opt/leak.
    
    mv bosch_cyber_tools.zip /opt/leak : Cette commande déplace le fichier bosch_cyber_tools.zip dans le répertoire /opt/leak.
    
    rm /tmp/mypassword : Cette commande supprime le fichier /tmp/mypassword, qui contenait le mot de passe pour l'archive ZIP.
    
    cd .. : Cette commande change le répertoire de travail courant au répertoire parent.
    
    rm -rf /home/b0sch/bosch_cyber_tools : Cette commande supprime de manière récursive le répertoire /home/b0sch/bosch_cyber_tools et son contenu, ce qui peut entraîner la suppression de données ou d'outils importants.
 
   Après avoir collecter toutes ces informations, nous avons procédé au tri afin de se concenter sur les données plus critiques. 
   nous avons donc accentuer nos analyses sur le fichier bosch_cyber_tools.zip afin de découvrir ce que l'attaquant a exfiltré. 
   On a commencé par récupèrer le zip chiffré en local : docker cp 2676834c4aa6:/opt/leak/bosch_cyber_tools.zip /home/kali/Documents
  
   On installe zip2john et john the ripper :
   
  git clone https://github.com/openwall/john -b bleeding-jumbo john
  cd john/src/
  ./configure
  make -s clean && make -sj4
  
  On execute zip2john :
  cd ../run/
  ./zip2john ../../Documents/bosch_cyber_tools.zip > bosch_cyber_tools.hash
  
 Puis john the ripper pour tenter de bruteforcer le mot de passe de l'archive :
john --wordlist=/usr/share/wordlists/rockyou.txt   bosch_cyber_tools.hash
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:03 DONE (2023-02-15 06:23) 0g/s 4612Kp/s 4612Kc/s 4612KC/s !LUVDKR!..*7¡Vamos!
Session completed. 

 le mot de passe trouvé ne semble pas fonctionner : unzip -P '!LUVDKR!..*7¡Vamos!' bosch_cyber_tools.zip
 incorrect password
 
 En essayant une autre approche, utiliser l'utilitaire testdisk pour essayer de retrouver le fichier /tmp/mypassword supprimé, il n'a pas été possible de pouvoir le récupérer.
 
 Recommandations
 
 afin qu'un pareil evènement ne se reprocuise plus, nous recommandons à cette entreprise les actions suivantes:     Utilisez des protocoles de sécurité : Assurez-vous que votre application utilise des protocoles de sécurité tels que HTTPS pour chiffrer les communications entre l'application et le serveur. Cela aidera à prévenir la divulgation de l'ID de l'utilisateur ou de l'ID de groupe de l'utilisateur courant lorsqu'ils sont transmis sur le réseau.

   - Gardez les informations confidentielles : Assurez-vous que l'ID de l'utilisateur et l'ID de groupe de l'utilisateur courant ne sont pas stockés de manière non sécurisée sur le serveur ou le client. Si ces informations doivent être stockées, assurez-vous qu'elles sont stockées de manière sécurisée, par exemple en les hashant ou en les encryptant.

   - Limitez l'accès aux informations : Restreignez l'accès à ces informations en utilisant des contrôles d'accès tels que des politiques de sécurité ou des rôles d'utilisateur. Assurez-vous que seules les personnes autorisées ont accès à ces informations.

   - Éduquez les utilisateurs : Informez les utilisateurs de l'importance de protéger leur ID de l'utilisateur et leur ID de groupe de l'utilisateur courant. Encouragez-les à utiliser des mots de passe forts et à ne pas divulguer ces informations à des tiers.

   - Suivez les meilleures pratiques de sécurité : Suivez les meilleures pratiques de sécurité telles que les mises à jour régulières de l'application, l'utilisation d'outils de détection des menaces et la mise en place de processus de réponse aux incidents.
   - 
 le fichier /etc/passwd est un fichier système important qui contient des informations sur les comptes d'utilisateurs du système. Il est donc crucial de sécuriser ce fichier pour empêcher toute personne non autorisée d'y accéder et d'obtenir des informations sensibles.

pour sécuriser le contenu du fichier /etc/passwd il faut:

   - Restreindre les autorisations d'accès au fichier : Le fichier /etc/passwd doit être accessible en lecture uniquement par le superutilisateur (root). Les autres utilisateurs ne devraient pas avoir accès en lecture au fichier.

   - Utiliser un système d'authentification robuste : Utilisez des systèmes d'authentification solides pour contrôler l'accès aux comptes d'utilisateur. Vous pouvez utiliser des méthodes d'authentification telles que les mots de passe forts, la double authentification, ou encore des clés SSH pour sécuriser l'accès aux comptes utilisateur.

   - Limiter l'accès aux commandes qui peuvent modifier le fichier : Seules les commandes nécessaires pour gérer les comptes utilisateur devraient être autorisées à modifier le fichier /etc/passwd. Vous pouvez utiliser les commandes de contrôle d'accès, tels que sudoers, pour contrôler les commandes accessibles.

   - Surveiller l'accès au fichier : Surveillez les accès au fichier /etc/passwd en utilisant des outils de journalisation tels que syslog. Les activités suspectes doivent être signalées pour pouvoir prendre des mesures immédiates.

   -Mmettre à jour régulièrement le système : Il est important de maintenir le système à jour avec les dernières mises à jour de sécurité pour prévenir les vulnérabilités connues qui pourraient permettre à des pirates d'accéder au fichier /etc/passwd.

Le fichier /etc/hosts est un fichier système qui contient des correspondances entre les adresses IP et les noms d'hôtes sur un système d'exploitation. Voici quelques étapes pour sécuriser le contenu de ce fichier afin qu'il ne soit tombe plus dans les mains des personnes malveillantes:

  *  Restreindre les autorisations d'accès : Comme pour le fichier /etc/passwd, il faut limiter l'accès en lecture et en écriture à ce fichier uniquement pour le superutilisateur.
  * Limiter les modifications du fichier : Les modifications apportées au fichier /etc/hosts ne doivent être effectuées que par un utilisateur de confiance, car un mauvais paramétrage peut entraîner une perte de connectivité réseau. Il est également recommandé de faire une copie de sauvegarde du fichier avant toute modification.

  * Éviter l'utilisation de noms d'hôtes non fiables : Évitez d'utiliser des noms d'hôtes provenant de sources non fiables ou inconnues, car cela peut entraîner des attaques de type phishing ou d'autres types d'attaques.

  * Surveiller les modifications du fichier : Surveillez les modifications apportées au fichier /etc/hosts en utilisant des outils de journalisation tels que syslog. Les activités suspectes doivent être signalées pour pouvoir prendre des mesures immédiates.

  * Configurer un pare-feu : Configurez un pare-feu pour bloquer toute tentative d'accès non autorisée au système d'exploitation. Cela peut aider à réduire le risque d'attaques provenant de l'extérieur.
  
  Dans le futur, il faudrait penser aux bons reflexes en cas d'attaque à savoir:
  - couper toute source d'alimentation électrique des équipements suspects
  - faire une image bit à bit du disque
  - déconnecter les postes du réseau pour éviter une propagation
  - dumper la RAM de la VM au cas où ça le poste affecté en est une
  - Documenter les évènements
  - avoir un double stockage pour la redondance des données
  - chiffrer les copies si ce sont des disques dur externes
  - prioriser l'utilisation du système de fichier NTFS car il est plus sécurisé et a un meilleur espace de stockage  
