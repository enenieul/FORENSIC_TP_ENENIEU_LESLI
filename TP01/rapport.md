l'analyse forensque de notre TP consiste à ananlyser un fichier winrar. pour se faire, nous allons suivre les étapes suivantes:l'identification du fichier pour déterminer son type et son format, il est de type winrar.car nous avons examiner son en-tête. ensuiste nous allons extraire les métadonnées telle que sa taille, sa date de création, son auteur et sa date de modification. puis nous allons ananalyser le contenu grace à des outils d'extraction et enfin nous allons rechercher les informations supprimées, analyser les données et preserver l'original du fichier car nous allons travailler avec la copie que nous avons fait préalablement


commandes :
apt update
apt install foremost
cd /home/lesli/usb-evidence
foremost usb-image ==> dossier output généré avec les images et le flag extraits
