# LAB 4 : Analyse statique d'un APK avec JADX GUI + dex2jar + JD-GUI

🎯 Objectif

Ce lab a pour objectif d’analyser statiquement une application Android (APK) afin de comprendre sa structure interne, identifier d’éventuelles vulnérabilités et comparer différents outils d’analyse comme JADX GUI, dex2jar et JD-GUI.

Task 1 — Préparer le workspace et vérifier l'APK

1- Créez un dossier de travail pour ce lab :
<img width="767" height="210" alt="image" src="https://github.com/user-attachments/assets/2e9799e9-5ac5-4e17-b15c-e8634abb41e2" />

2- Copiez l'APK à analyser dans ce dossier.
<img width="950" height="279" alt="image" src="https://github.com/user-attachments/assets/41741edb-92e0-4a5f-8490-3cee9acc44ec" />

3- Vérifiez que l'APK est bien une archive ZIP :
<img width="680" height="300" alt="image" src="https://github.com/user-attachments/assets/5d5604cd-f891-490f-a759-c364fe834775" />

4- Listez le contenu de l'APK :
<img width="829" height="218" alt="image" src="https://github.com/user-attachments/assets/9e47a250-41ed-4fc4-9adf-4d402e8789ec" />

5- Calculez le hash de l'APK pour traçabilité :
<img width="843" height="82" alt="image" src="https://github.com/user-attachments/assets/35cde1ed-82f8-428b-8a92-bc513688ec5e" />

6- (Optionnel) Vérifiez la signature de l'APK :
<img width="947" height="181" alt="image" src="https://github.com/user-attachments/assets/fb92983b-1e51-46b3-9a59-08e48a6ce0ab" />

Task 3 — Analyse avec JADX GUI

<img width="959" height="470" alt="image" src="https://github.com/user-attachments/assets/d7dfcd4d-6e85-42fc-9fff-b5b8fefe0a11" />

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/6bf9b844-28f5-4a53-8077-3c7fb96074a1" />
1. Package principal, versionName, minSdk, targetSdk


Package principal : owasp.mstg.uncrackable1


versionName : 1.0


versionCode : 1


minSdkVersion : 19


targetSdkVersion : 28



2. Permissions demandées


Aucune permission n’est déclarée dans le manifeste.


Il n’y a pas de balise <uses-permission>.



3. Composants identifiés


Activity : sg.vantagepoint.uncrackable1.MainActivity


Services : aucun


Receivers : aucun


Providers : aucun



4. Composants exportés ou avec intent-filter


MainActivity possède un intent-filter avec :


android.intent.action.MAIN


android.intent.category.LAUNCHER




Donc c’est l’activité principale lancée au démarrage de l’application.

5. usesCleartextTraffic / debuggable


android:usesCleartextTraffic="true" : absent


android:debuggable="true" : absent


Donc aucune configuration explicite dangereuse de ce type n’est visible dans le manifeste.

Explorez les ressources importantes :
strings.xml pour les chaînes de caractères

<img width="959" height="426" alt="image" src="https://github.com/user-attachments/assets/321754c4-0d69-41aa-90cf-99984e131493" />
Le fichier strings.xml a été exploré afin d’identifier les chaînes de caractères utilisées par l’application. Cette analyse permet de repérer d’éventuels secrets en clair, messages sensibles, URLs ou clés API exposées.
Le fichier strings.xml a été analysé afin d’identifier les chaînes de caractères utilisées par l’application. Aucune information sensible telle que des mots de passe, clés API ou tokens n’a été trouvée. Toutefois, la présence du message “Enter the Secret String” indique que l’application repose sur un mécanisme de vérification basé sur une valeur secrète, suggérant que cette logique est implémentée dans le code source.

network_security_config.xml si présent
Autres fichiers XML de configuration
Aucun fichier network_security_config.xml n’a été trouvé dans les ressources de l’APK. Les autres fichiers XML identifiés concernent principalement l’interface graphique (activity_main.xml), le menu (menu_main.xml) et les ressources textuelles ou visuelles (strings.xml, styles.xml, dimens.xml). Aucun fichier de configuration réseau sensible n’a été observé.

Task 4 — Recherche de chaînes sensible
Observation 1
Recherche : http://
Résultat : Aucun résultat trouvé
Emplacement : N/A
Risque : Faible
Description : Aucune URL non sécurisée n’a été détectée dans le code de l’application.
<img width="587" height="367" alt="image" src="https://github.com/user-attachments/assets/dfa40398-4946-4f84-a3f6-69f16e677067" />

.io
<img width="480" height="499" alt="image" src="https://github.com/user-attachments/assets/debaf635-45df-40c7-abf9-d736220553fa" />

Recherchez des informations d'authentification :
secret
<img width="477" height="498" alt="image" src="https://github.com/user-attachments/assets/3e48fcd0-aea8-49e6-acab-9c61dc6090b8" />

Recherchez des indicateurs de mode de développement :
<img width="478" height="470" alt="image" src="https://github.com/user-attachments/assets/0246a0d6-3e53-4da2-9259-17fe13c75177" />
<img width="476" height="449" alt="image" src="https://github.com/user-attachments/assets/b8c1ddc0-415d-497f-9ec2-dab862ff7bfc" />
<img width="691" height="335" alt="image" src="https://github.com/user-attachments/assets/174f6c79-0946-4619-9d91-55a3793f463f" />
<img width="951" height="337" alt="image" src="https://github.com/user-attachments/assets/1287d398-5189-42a5-b9a9-5af2208b8797" />

Task 5 — Convertir DEX → JAR avec dex2jar (15-20 min)
<img width="479" height="131" alt="image" src="https://github.com/user-attachments/assets/e7c638df-c1e0-485e-8c68-400a631721db" />
<img width="812" height="14" alt="image" src="https://github.com/user-attachments/assets/ca31c4ac-da11-445b-9537-fba026a67ace" />
<img width="689" height="62" alt="image" src="https://github.com/user-attachments/assets/34c2b8be-fbf4-4412-869d-3623ba15ebc8" />
<img width="608" height="122" alt="image" src="https://github.com/user-attachments/assets/54df719b-8e97-430d-82e3-6fb0ee9b67a2" />
<img width="850" height="40" alt="image" src="https://github.com/user-attachments/assets/7609b253-822e-41c2-8b25-b90a7bba9ff7" />

🎯 Task 6 — Comparaison JADX vs JD-GUI

<img width="437" height="292" alt="image" src="https://github.com/user-attachments/assets/3677d8af-1c7f-43a3-a194-df1a930ed5f3" />
Comparez la même classe/méthode dans JADX GUI et JD-GUI :
Navigation et organisation
Lisibilité du code
Gestion des constructions spécifiques à Android
Impact de l'obfuscation
Affichage des ressources













