# Burp

# H4CK3RZ :
## Présentation Etapes par Etapes de la récupération du flag :
- Nmap pour détecter les ports : 22 (SSH) et 80 (HTTP).
- Découverte du site, exploration de son code source ; Découverte - en commentaire - d'un nom d'utilisateur.
- Utilisation de gobuster pour trouver les pages de base d'Apache2 : .htaccess, .htpasswd (qui ne donnent rien car pas d'accès) et robots.txt : Bingo, un chemin d'URL dedans.
- Sur le chemin d'URL, récupération d'un mot de passe inscrit en clair. Extraction du nom d'utilisateur et mot de passe.$
- Réutilisation de gobuster pour trouver les pages html et php de base : Page login.php trouvée.
- Connexion sur login.php avec les identifiants récupérés avant. Accès à 42sh assuré.
- Mise en place d'une connexion sh dans le 42sh à mon terminal grâce à [RevShells](https://www.revshells.com/) qui a permis de créer un tunnel pour accéder au terminal de l'autre machine : Technique du Reverse Shell.
- Extraction dans le dossier source `/var/www/html` du **mandatory_flag** et dans le `/home/titouan` pour le **optional_flag**.$
- Tentative infructueuse de téléchargement de LSE et Linpeas via `Wget` et `git clone` mais freeze de la machine.

# Batman's Secret :
## Présentation Etapes par Etapes de la récupération du flag :
- Nmap de l'IP : 3 ports trouvés, 21 (FTP), 22 (SSH), 3000 (Web).
- Exploration de FTP : Extraction de `alert.py` et d'un fichier `report.txt`.
- Ncat sur le Port 3000 et découverte de l'exécution du code d'`alert.py` une fois exécuté (sur la machine web).
- Injection XSS pour introduire un Reverse Shell dans ce code, grâce à l'exécution d'un code peu sécurisé `os.system("bash -c 'echo %s >> /opt/hotline/logs/report.txt'" % alert_text)` <sub>Le code précédent a la particularité de contenir une faille énorme, en utilisant "bash" dans le code. Ce qui a ouvert la possibilité, comme dans une Injection SQL par séparation, l'introduction d'un `;` de séparation, et de caractères `'` de commentaires pour affirmer la séparation de la commande 1 (bash -c 'echo') et de la commande 2 (celle qu'on introduit dans le code)</sub>
- Récupération de `user.txt` dans /home/bruce_wayne et extraction du contenu du fichier, récupération alors du mandatory_flag.
- Tenative d'un second RevShell en sudo -u bruwe_wayne, infructueux, crash machines répétitifs. (possibilité que sudo soit bloqué ou demande un password).