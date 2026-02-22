# LAB-4-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI

# Rapport d'analyse statique - DivaApplication

## Informations générales
- **Date d'analyse :** 22/02/2026
- **Analyste :** Mohamed Douassi
- **APK analysé :** DivaApplication.apk
- **Version :** 1.0
- **Provenance :** DIVA (Damn Insecure and Vulnerable App)
- **Outils utilisés :** JADX GUI v1.5.4, dex2jar v2.4, JD-GUI v1.6.6

---

## Résumé exécutif

Cette analyse statique a révélé **4 vulnérabilités potentielles** dans l'application DivaApplication.

Les principales préoccupations concernent :
- Un secret hardcodé dans le code source
- Un Content Provider exporté sans protection
- Un mode debug activé en production

Le niveau de risque global est évalué comme **ÉLEVÉ**.

### Actions prioritaires recommandées :
1. Supprimer immédiatement les secrets codés en dur.
2. Désactiver ou protéger le Content Provider exporté.
3. Désactiver le mode debug dans la version de production.

---

## Constats détaillés

### Constat #1 : Secret hardcodé dans le code source

**Sévérité :** Élevée  

<img width="673" height="356" alt="image" src="https://github.com/user-attachments/assets/f963eea4-39a8-43d2-bb73-b4d125a203b6" />

**Description :** Une clé secrète est directement intégrée dans le code source et comparée à l’entrée utilisateur pour autoriser l’accès.  
**Localisation :** HardcodeActivity.java — méthode `access()`  
**Impact potentiel :** Un attaquant peut décompiler l’APK, récupérer la clé secrète et contourner le mécanisme d’authentification.  
**Remédiation recommandée :** Stocker les secrets dans Android Keystore ou sur un serveur distant sécurisé. Ne jamais hardcoder de données sensibles.

---

### Constat #2 : Content Provider exporté sans protection

<img width="580" height="105" alt="image" src="https://github.com/user-attachments/assets/16342acd-75ce-4ad7-bb71-74b64659943f" />

**Sévérité :** Élevée  
**Description :** Le composant `NotesProvider` est déclaré avec `android:exported="true"` dans le manifeste.  
**Localisation :** AndroidManifest.xml  
**Impact potentiel :** Toute application tierce installée sur l’appareil peut accéder aux données stockées (lecture, modification, suppression).  
**Remédiation recommandée :** Définir `android:exported="false"` ou protéger le composant avec une permission appropriée.

---

### Constat #3 : Mode debug activé

<img width="531" height="145" alt="image" src="https://github.com/user-attachments/assets/77d663f8-c9a1-420e-874e-211c365abefc" />

**Sévérité :** Moyenne  
**Description :** L’application est configurée avec `android:debuggable="true"`.  
**Localisation :** AndroidManifest.xml — balise `<application>`  
**Impact potentiel :** Possibilité d’attacher un débogueur, d’extraire des informations sensibles et d’analyser le comportement interne.  
**Remédiation recommandée :** Désactiver le mode debug en production et utiliser une configuration de build release.

---

### Constat #4 : Indices d’attaque dans les ressources

<img width="1029" height="198" alt="image" src="https://github.com/user-attachments/assets/3a247e05-8ab9-4645-9d83-a7f11ea29050" />

<img width="1044" height="159" alt="image" src="https://github.com/user-attachments/assets/1ca6fbc2-7e9e-44e0-b719-e16ce8e54d9b" />

 <string name="notesprovider_url">content://jakhar.aseem.diva.provider.notesprovider/notes</string>


**Sévérité :** Faible (Informative)  
**Description :** Le fichier strings.xml contient des indications facilitant l’exploitation des vulnérabilités.  
**Localisation :** res/values/strings.xml  
**Impact potentiel :** Simplifie le travail d’un attaquant en révélant des informations internes.  
**Remédiation recommandée :** Supprimer les messages d’aide et indices dans les versions de production.

---

## Annexes

### Permissions demandées
- `WRITE_EXTERNAL_STORAGE`
- `READ_EXTERNAL_STORAGE`
- `INTERNET`

<img width="633" height="59" alt="image" src="https://github.com/user-attachments/assets/d61be17f-aade-4e2b-bcb4-f9faa1463925" />

### Composants exportés
- `MainActivity`
- `APICredsActivity`
- `APICreds2Activity`
- `NotesProvider`
