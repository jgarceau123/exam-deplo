Sur ton serveur ServPretest :
1. Ouvre l'Explorateur de fichiers.
2. Va dans C:\.
3. Crée un dossier nommé Installateurs.
4. Déplace tes 5 installateurs dans ce dossier.
5. Clic droit sur Installateurs → Propriétés.
6. Va dans Partage → Partage avancé.
7. Coche Partager ce dossier.
8. Nomme le partage Installateurs.
Tu obtiendras ce chemin :
\\ServPretest\Installateurs



1. Préparer PhotoFiltre
Place ton installateur dans le dossier partagé du serveur, par exemple :
\\ServPretest\Installateurs\PhotoFiltre.exe
Le nom du fichier doit correspondre exactement à celui que tu as téléchargé.
2. Installation silencieuse
Ton professeur demande ces deux commutateurs :
/VERYSILENT /SUPPRESSMSGBOXES


Voici un exemple de commande PowerShell :
Start-Process -FilePath "\\ServPretest\Installateurs\PhotoFiltre.exe" -ArgumentList "/VERYSILENT /SUPPRESSMSGBOXES" -Wait


Cette commande lance l'installateur sans afficher l'assistant d'installation et attend la fin du processus, si l'installateur prend bien en charge ces commutateurs.


3. Créer la GPO
Dans Gestion des stratégies de groupe :
1. Clic droit sur ton OU PRETEST.
2. Choisis Créer un objet GPO dans ce domaine, et le lier ici.
3. Nomme-le Deploy_PhotoFiltre.
4. Clic droit sur la GPO → Modifier.
5. Va dans :
Configuration ordinateur → Stratégies → Paramètres Windows → Scripts (démarrage/arrêt) → Démarrage
Dans l'onglet Scripts PowerShell, tu pourras ajouter ton script .ps1.
Important : ton examen précise qu'un script PowerShell t'est fourni et que tu dois modifier ses variables. Il vaut donc mieux utiliser ce script plutôt que d'en créer un nouveau.



Parfait ! On va créer les 4 GPO pour les logiciels MSI en premier. On gardera PhotoFiltre pour la fin.
Les GPO à créer
Nom de la GPO	Logiciel	Type
Deploy_Firefox	Firefox	Publié
Deploy_Inkscape	Inkscape	Publié
Deploy_NotepadPP	Notepad++	Publié
Deploy_VLC	VLC	Publié
Toutes doivent être dans la Configuration utilisateur et liées à ton OU PRETEST.
Étape 1 — Créer Deploy_Firefox
Sur ton serveur Windows Server 2022 :
1. Appuie sur Windows + R.
2. Écris gpmc.msc et appuie sur Entrée.
3. Développe Forêt → Domaines → PRETEST.local.
4. Fais un clic droit sur ton OU PRETEST.
5. Sélectionne Créer un objet GPO dans ce domaine, et le lier ici.
6. Nomme la GPO Deploy_Firefox.
7. Clique sur OK.

8. Étape 2 — Configurer la GPO
1. Clic droit sur Deploy_Firefox → Modifier.
2. Navigue jusqu'à :
 Configuration utilisateur

 Stratégies

 Paramètres du logiciel

 Installation de logiciel





3. Clic droit sur Installation de logiciel.
4. Sélectionne Nouveau → Package.
5. Entre le chemin réseau de ton installateur Firefox, par exemple :
\\ServPretest\installateurs\Firefox Setup 157.0.1.msi
6. Sélectionne Publié.
7. Clique sur OK.
Le nom et l'extension du fichier doivent correspondre exactement à ton installateur. Le chemin UNC doit être accessible depuis le poste client.




Étape 3 — Configurer le filtrage de sécurité
1. Retourne dans Gestion des stratégies de groupe.
2. Clique sur Deploy_Firefox.
3. Dans l'onglet Étendue, trouve Filtrage de sécurité.
4. Retire Utilisateurs authentifiés du filtrage.
5. Clique sur Ajouter et sélectionne Groupe_Firefox.
6. Dans Délégation → Avancé, assure-toi que Utilisateurs authentifiés conserve Lecture, sans Appliquer la stratégie de groupe.
Ainsi, seuls les membres du groupe Firefox pourront appliquer cette GPO.





La différence importante :
- Lire : permet de consulter la stratégie.
- Appliquer la stratégie de groupe : permet à la GPO de s'appliquer à l'utilisateur ou à l'ordinateur concerné.
Il faut normalement les deux autorisations pour que la GPO s'applique.
À retenir pour ton examen
Tu retires seulement Appliquer la stratégie de groupe pour Utilisateurs authentifiés, pas Lire.
Ensuite, tu donnes les deux autorisations à Groupe_Firefox.
De cette façon, ta GPO deploy_firefox sera ciblée uniquement sur les membres du groupe Firefox, à condition qu'ils soient dans la portée de la GPO.
C'est exactement ce que ton professeur demande lorsqu'il parle de filtrage de sécurité.



pour la GPO de .exe CONFIG ORDI ---- para windows ---- script ---- demarage ---- METRE LE SCRIPT PS SUR LE BUREAU. --- CLIQUER SUR AJOUTER --- PARCOURIRE --- GLISSER LE SCRIT DU BUREAU A LA... -- PLUS BAS EXECUTER LES SCRIPT WINDOWS PS EN PREMIER
