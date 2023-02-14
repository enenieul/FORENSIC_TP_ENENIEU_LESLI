L'analyse forensique du TP consiste à analyser un fichier de type image disque (dump d'une clé USB). Pour ce faire, je vais suivre les étapes suivantes :
- Procéder à l'identification du fichier pour déterminer son type et son format. 
- Ensuite je vais extraire les métadonnées telle que sa taille, sa date de création, son auteur et sa date de modification. 
- Enfin, je vais analyser le contenu grace à des outils d'extraction préserver le fichier original en travaillant avec la copie que nous avons fait préalablement.

Pour extraire le contenu de notre fichier, j'ai suivi les étapes suivantes:

Prérequis : installation d'une VM Kali Linux

- Mise à jour des listes de paquets :
  apt update
  
- j'ai installé foremost qui est l'outil que j'ai choisi pour cette analyse.
  apt install foremost
  
- Puis je me suis rendue dans le terminal dans le repertoire où j'ai téléchargé le dossier de preuves
  cd /home/lesli/usb-evidence
  
- Ensuite j'ai utilisé la commande suivante pour extraire les données
  foremost usb-image ==> cela a généré un dossier output avec les images et le flag extraits visible dans ce repository dans le dossier du même nom.

