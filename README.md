# LAB-5-Reverse-Engineering-de-UnCrackable-Level-2
1. Introduction
L’objectif de ce TP est de réaliser l’ingénierie inverse de l’application Android "Uncrackable Level 2" afin d’extraire un secret stocké de manière sécurisée. Contrairement au niveau 1, la vérification du code se fait au niveau natif (C/C++).
<img width="335" height="773" alt="image" src="https://github.com/user-attachments/assets/90feff7a-3158-448f-b703-9fa89b2f101d" />
-	L'application a été installée avec succès via ADB, mais elle se ferme immédiatement après l'ouverture. Cela indique une protection active contre le Root ou le Debugging
-
-	 2. Analyse Statique (JADX-GUI)
D'abord, j'ai ouvert l'APK avec JADX-GUI pour identifier la logique de vérification.

Observation : La classe CodeCheck appelle une méthode native nommée bar.

Indice : Cette méthode est chargée depuis une bibliothèque native via System.loadLibrary("foo").

Action : Extraction du fichier libfoo.so depuis le dossier lib/x86 (ou arm) de l'APK.


3. Analyse Native avec Ghidra
Pour analyser le code binaire, j'ai utilisé Ghidra 12.0.4.

Installation : Configuration de l'environnement avec JDK 21 pour lancer ghidraRun.bat.

Importation : Chargement de libfoo.so et lancement de l'analyse automatique.

Recherche : Localisation de la fonction JNI : Java_sg_vantagepoint_uncrackable2_CodeCheck_bar.

Décompilation : Le pseudo-code révèle l'utilisation de la fonction strncmp pour comparer l'entrée utilisateur avec une variable locale.

Capture d'écran conseillée : L'interface de Ghidra avec le code de la fonction bar et la ligne strncmp.
<img width="1214" height="566" alt="2" src="https://github.com/user-attachments/assets/7866dc52-c63b-4fef-a336-e86deec37b02" />


4. Décodage du Secret (CyberChef)
Le secret identifié dans Ghidra apparaît sous forme hexadécimale en raison du stockage en mémoire (Little Endian).

Valeur Hex : 6873696620656874206c6c6120726f6620736b6e616854

Outil : Utilisation de CyberChef.

Recette :

From Hex

Reverse

Résultat : Thanks for all the fish

<img width="683" height="680" alt="3" src="https://github.com/user-attachments/assets/7dfd6171-8389-45cf-bf2c-a816e8e767e4" />

5. Validation Finale
En saisissant la chaîne Thanks for all the fish dans l'application :

Résultat : L'application affiche un message de Success.

Conclusion : Le secret a été extrait avec succès malgré l'obscurcissement au niveau natif.5. Validation Finale
En saisissant la chaîne Thanks for all the fish dans l'application :

Résultat : L'application affiche un message de Success.

Conclusion : Le secret a été extrait avec succès malgré l'obscurcissement au niveau natif.
