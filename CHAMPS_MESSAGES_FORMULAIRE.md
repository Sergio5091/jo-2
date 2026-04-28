# 📋 Champs Requis et Messages du Formulaire

## 🎯 **Vue d'ensemble du Formulaire**
Le formulaire est divisé en **4 étapes** avec validation progressive et messages d'erreur spécifiques.

---

## 📝 **Étape 1: Identité (StepIdentity.tsx)**

### 🏷️ **Champs Requis**
1. **ID Employé** 
   - **Type** : Texte
   - **Placeholder** : "5485123"
   - **Validation** : Obligatoire
   - **Format attendu** : 4568721425

2. **Nom de famille**
   - **Type** : Texte  
   - **Placeholder** : "KOUASSI"
   - **Validation** : Obligatoire
   - **Label** : "Nom de famille"

3. **Prénom**
   - **Type** : Texte
   - **Placeholder** : "Jean"
   - **Validation** : Obligatoire
   - **Label** : "Prénom"

4. **Téléphone mobile**
   - **Type** : Téléphone
   - **Placeholder** : "+225 0700000000"
   - **Validation** : Obligatoire
   - **Valeur par défaut** : "+225 "

5. **Nom complet** (Contact urgence)
   - **Type** : Texte
   - **Placeholder** : "TRAORÉ Marie"
   - **Validation** : Obligatoire
   - **Label** : "Nom complet"

6. **Téléphone** (Contact urgence)
   - **Type** : Téléphone
   - **Placeholder** : "+225 0700000000"
   - **Validation** : Obligatoire
   - **Label** : "Téléphone"

### 💬 **Messages d'Erreur Étape 1**
```javascript
"Veuillez remplir tous les champs obligatoires de l'étape Identité avant de continuer."
```

### 🎨 **Design**
- **Section urgence** : Séparée par ligne bordure grise
- **Champs** : Style uniforme avec focus gris
- **Labels** : Gris foncé avec astérisque rouge pour obligatoire

---

## 📅 **Étape 2: Disponibilité (StepAvailability.tsx)**

### 🏷️ **Champs Requis**
1. **Problème de santé**
   - **Type** : Textarea (3 lignes)
   - **Placeholder** : "Décrivez brièvement tout problème de santé pertinent..."
   - **Validation** : Optionnel
   - **Label** : "Problème de santé (optionnel)"

2. **Date de disponibilité**
   - **Type** : Date
   - **Validation** : Obligatoire
   - **Label** : "Date de disponibilité *"

### 💰 **Information Rémunération**
- **Indemnité de stage** : 40 000 – 80 000 FCFA
- **Salaire CDI** : 100 000 – 250 000 FCFA
- **Design** : Carte grise avec informations

### 💬 **Messages d'Erreur Étape 2**
```javascript
"Veuillez sélectionner votre date de disponibilité avant de continuer."
```

---

## 💳 **Étape 3: CNPS (StepCNPS.tsx)**

### 🏷️ **Champs Conditionnels**

#### **Si OUI (A une carte CNPS)**
1. **Numéro CNPS**
   - **Type** : Texte
   - **Placeholder** : "Ex : CI-2024-00000"
   - **Validation** : Obligatoire si hasCNPS = true
   - **Label** : "Numéro CNPS *"

#### **Si NON (N'a pas de carte CNPS)**
1. **Preuve de paiement**
   - **Type** : Upload fichier
   - **Formats** : JPG, PNG, PDF
   - **Validation** : Obligatoire si hasCNPS = false
   - **Taille** : Affichage en KB
   - **Coût** : 14 500 FCFA

2. **Pièce d'identité**
   - **Type** : Upload fichier
   - **Formats** : JPG, PNG, PDF
   - **Validation** : Obligatoire si hasCNPS = false
   - **Type** : Recto/Verso
   - **Design** : Drag & drop avec prévisualisation

### 💬 **Messages d'Erreur Étape 3**
```javascript
// Si pas de sélection CNPS
"Veuillez sélectionner 'Oui' ou 'Non' pour la carte CNPS avant de continuer."

// Si CNPS = OUI mais pas de numéro
"Veuillez entrer votre numéro CNPS avant de continuer."

// Si CNPS = NON mais fichiers manquants
"Vous devez ajouter la preuve de paiement de 14500 FCFA pour permettre la création de votre carte CNPS avant de continuer."
"Vous devez ajouter votre pièce d'identité pour permettre la création de votre carte CNPS avant de continuer."
```

### 🎨 **Design Upload**
- **Zone vide** : Bordure pointillée grise
- **Drag active** : Bordure grise foncée
- **Fichier uploadé** : Bordure solide avec aperçu
- **Bouton suppression** : Croix X grise

---

## ✅ **Étape 4: Engagement (StepEngagement.tsx)**

### 📋 **Récapitulatif Affiché**
- **Nom complet** : `${firstName} ${fullName}`
- **ID Employé** : Format SIR-03-XXX
- **Téléphone** : Format +225 XXXXXXXXX
- **Disponibilité** : Date format YYYY-MM-DD
- **Carte CNPS** : Oui/Non/—
- **N° CNPS** : Si applicable
- **Preuve de paiement** : Nom du fichier si applicable

### 🏷️ **Champs Requis**
1. **Confirmation exactitude**
   - **Type** : Checkbox
   - **Texte** : "Je confirme l'exactitude des informations fournies..."
   - **Validation** : Obligatoire
   - **Design** : Checkbox personnalisée avec coche SVG

2. **Acceptation conditions**
   - **Type** : Checkbox
   - **Texte** : "J'accepte les conditions générales d'utilisation..."
   - **Validation** : Obligatoire
   - **Design** : Checkbox personnalisée avec coche SVG

### 💬 **Messages d'Information Étape 4**
```javascript
// Si checkboxes non cochées
"Veuillez cocher les deux cases pour soumettre votre dossier."
```

### 🎨 **Design Récapitulatif**
- **Carte grise** : Fond #F9FAFB avec bordure grise
- **Lignes** : Alternance label gris/valeur noir
- **Checkboxes** : Design personnalisé avec animations
- **Message final** : Texte informatif en gris clair

---

## 🚨 **Modal d'Erreurs Général**

### 🎨 **Design Modal**
- **Overlay** : Fond noir 40% avec backdrop blur
- **Boîte** : Fond blanc, bordure rouge, coins arrondis
- **Icône** : Cercle rouge avec "!" blanc
- **Titre** : "Documents manquants"

### 📝 **Messages Affichés**
```javascript
// Liste des erreurs possibles
[
  "Veuillez remplir tous les champs obligatoires de l'étape Identité avant de continuer.",
  "Veuillez sélectionner votre date de disponibilité avant de continuer.",
  "Veuillez sélectionner 'Oui' ou 'Non' pour la carte CNPS avant de continuer.",
  "Veuillez entrer votre numéro CNPS avant de continuer.",
  "Vous devez ajouter la preuve de paiement de 14500 FCFA pour permettre la création de votre carte CNPS avant de continuer.",
  "Vous devez ajouter votre pièce d'identité pour permettre la création de votre carte CNPS avant de continuer."
]
```

### 🎯 **Comportement Modal**
- **Ouverture** : Automatique sur erreur de validation
- **Fermeture** : Clic sur overlay ou bouton "Compris"
- **Messages** : Listes à puces rouges sur fond rouge clair
- **Bouton** : "Compris" en gris foncé

---

## 📧 **Messages de Soumission**

### ✅ **Succès**
```javascript
// Console logs
"✅ EmailJS - Email envoyé avec succès !"

// Page succès
"Dossier soumis avec succès"
"Votre candidature a bien été enregistrée. Vous serez contacté(e) par la Direction des Ressources Humaines dans les plus brefs délais."
```

### ❌ **Erreur Envoi**
```javascript
// Console erreur
"Erreur lors de l'envoi de l'email:", [error details]

// Message utilisateur
"Erreur lors de l'envoi du formulaire. Veuillez réessayer."
```

---

## 📱 **Messages d'Aide et Instructions**

### 📋 **Instructions Étape 1**
```
Étape 1 sur 4 — Informations personnelles
```

### 📅 **Instructions Étape 2**
```
Étape 2 sur 4 — Date de prise de poste
```

### 💳 **Instructions Étape 3**
```
Étape 3 sur 4 — Caisse Nationale de Prévoyance Sociale
```

### ✅ **Instructions Étape 4**
```
Étape 4 sur 4 — Confirmation finale
```

### 🔒 **Message Confidentialité**
```
Vos informations sont traitées de manière strictement confidentielle.
```

---

## 🎯 **Validation Globale**

### ✅ **Champs Obligatoires Finale**
Pour soumettre le formulaire, TOUS ces éléments doivent être validés :
1. ✅ ID Employé (SIR-03-XXX)
2. ✅ Nom de famille
3. ✅ Prénom
4. ✅ Téléphone personnel
5. ✅ Nom contact urgence
6. ✅ Téléphone urgence
7. ✅ Date de disponibilité
8. ✅ Choix CNPS (Oui/Non)
9. ✅ Numéro CNPS (si OUI)
10. ✅ Preuve paiement CNPS (si NON)
11. ✅ Pièce d'identité (si NON)
12. ✅ Confirmation exactitude (checkbox)
13. ✅ Acceptation conditions (checkbox)

### 🚫 **Boutons Désactivés Si**
- **"Suivant"** : Champs obligatoires non remplis
- **"Soumettre"** : Checkboxes non cochées ou envoi en cours

---

## 📞 **Coordonnées Support Affichées**

### 📧 **Email**
```
supportrecrut@gmail.com
```

### 📱 **Téléphone**
```
WhatsApp / Wave : 2250767554788
```

### 💰 **Paiement CNPS**
```
Paiement via WhatsApp / Wave : 225076755478
Frais de dossier CNPS : 14 500 FCFA
```

---

voila les information du .env a utiliser 

# Configuration EmailJS pour l'envoi des formulaires par email
# Remplacez ces valeurs par vos vraies clés EmailJS

# Configuration du serveur de développement
PORT=5174
BASE_PATH=/

# Email destinataire des formulaires
VITE_RECIPIENT_EMAIL=supportrecrut@gmail.com

# Votre Service ID EmailJS (trouvé dans Email Services)
VITE_EMAILJS_SERVICE_ID=service_1wldsfh

# Votre Template ID EmailJS (trouvé dans Email Templates)
VITE_EMAILJS_TEMPLATE_ID=template_1sck38t

# Votre clé publique EmailJS (trouvé dans Account > General)
VITE_EMAILJS_PUBLIC_KEY=s-hRBpkrIqMHNqlQs