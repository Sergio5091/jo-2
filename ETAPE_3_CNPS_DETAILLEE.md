# 💳 Étape 3: CNPS - Description Détaillée

## 🎯 **Vue d'ensemble de l'Étape 3**
L'étape CNPS gère la Caisse Nationale de Prévoyance Sociale avec **logique conditionnelle** selon que l'utilisateur ait déjà une carte CNPS ou non.

---

## ✅ **CAS 1: UTILISATEUR A UNE CARTE CNPS (Clique sur "OUI")**

### 🎨 **Interface Affichée**
```
┌─────────────────────────────────────────────┐
│ CNPS                                   │
│ Étape 3 sur 4 — Caisse Nationale    │
│ de Prévoyance Sociale               │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ Avez-vous une carte CNPS ? *           │
│ [ OUI ]  [ NON ]                    │ <- Bouton OUI sélectionné (fond noir)
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ Numéro CNPS *                         │
│ ┌─────────────────────────────────────┐   │
│ │ Ex : CI-2024-00000             │   │
│ └─────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

### 🏷️ **Champ Requis**
- **Label** : "Numéro CNPS *"
- **Type** : Input texte
- **Placeholder** : "Ex : CI-2024-00000"
- **Validation** : Obligatoire
- **Format attendu** : CI-AAAA-NNNNN

### 🎨 **Design Visuel**
- **Bouton OUI** : Fond gris foncé (#111827), texte blanc
- **Bouton NON** : Fond blanc, bordure grise, texte gris
- **Champ numéro** : Fond blanc, bordure grise, focus gris
- **Layout** : Champ seul, centré, largeur complète

### 📋 **Logique de Validation**
```javascript
// Si hasCNPS = true
if (formData.hasCNPS === true) {
  // Le numéro CNPS devient obligatoire
  if (!formData.cnpsNumber || formData.cnpsNumber.trim() === '') {
    // Message d'erreur
    "Veuillez entrer votre numéro CNPS avant de continuer."
  }
}
```

---

## ❌ **CAS 2: UTILISATEUR N'A PAS DE CARTE CNPS (Clique sur "NON")**

### 🎨 **Interface Affichée**
```
┌─────────────────────────────────────────────┐
│ CNPS                                   │
│ Étape 3 sur 4 — Caisse Nationale    │
│ de Prévoyance Sociale               │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ Avez-vous une carte CNPS ? *           │
│ [ OUI ]  [ NON ]                    │ <- Bouton NON sélectionné (fond noir)
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ 🔶 Frais de dossier CNPS : 14 500 FCFA │
│                                      │
│ Ces frais correspondent à l'ouverture     │
│ et au traitement de votre dossier auprès    │
│ de la Caisse Nationale de Prévoyance     │
│ Sociale (CNPS) en Côte d'Ivoire.    │
│                                      │
│ Paiement via WhatsApp / Wave :            │
│ 225076755478                        │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ 📄 Preuve de paiement                  │
│ ┌─────────────────────────────────────┐   │
│ │    [Glissez fichier ici]        │   │ <- Zone vide au début
│ │         ou parcourir              │   │
│ │                                  │   │
│ │    JPG, PNG ou PDF              │   │
│ └─────────────────────────────────────┘   │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ 🆔 Pièce d'identité (Recto/Verso)   │
│ ┌─────────────────────────────────────┐   │
│ │    [Glissez fichier ici]        │   │ <- Zone vide au début
│ │         ou parcourir              │   │
│ │                                  │   │
│ │    JPG, PNG ou PDF - Recto/Verso │   │
│ └─────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

### 🏷️ **Champs Requis**
1. **Preuve de paiement**
   - **Type** : Upload fichier
   - **Formats** : JPG, PNG, PDF
   - **Validation** : Obligatoire
   - **Taille max** : Affichée en KB

2. **Pièce d'identité**
   - **Type** : Upload fichier
   - **Formats** : JPG, PNG, PDF
   - **Validation** : Obligatoire
   - **Spécification** : Recto/Verso

### 💰 **Information Paiement**
- **Montant** : 14 500 FCFA
- **Moyens** : WhatsApp / Wave
- **Contact** : 225076755478
- **Design** : Carte grise avec icône €

### 🎨 **Design Upload**
- **Zone vide** : Bordure pointillée grise
- **Drag active** : Bordure grise foncée, fond gris clair
- **Fichier uploadé** : Bordure solide, aperçu avec nom + taille
- **Bouton suppression** : Croix X grise
- **Icônes** : Upload (vide), FileText (uploadé), CreditCard (identité)

### 📋 **Logique de Validation**
```javascript
// Si hasCNPS = false
if (formData.hasCNPS === false) {
  const errors = [];
  
  // Vérification preuve de paiement
  if (!formData.cnpsProof) {
    errors.push(
      "Vous devez ajouter la preuve de paiement de 14500 FCFA pour permettre la création de votre carte CNPS avant de continuer."
    );
  }
  
  // Vérification pièce d'identité
  if (!formData.identityCard) {
    errors.push(
      "Vous devez ajouter votre pièce d'identité pour permettre la création de votre carte CNPS avant de continuer."
    );
  }
  
  // Si erreurs, bloquer la navigation
  if (errors.length > 0) {
    setModalErrors(errors);
    setShowErrorModal(true);
    return; // Bloquer la navigation
  }
}
```

---

## 🔄 **Comportement Interactif**

### 🖱️ **Drag & Drop**
- **DragOver** : Bordure grise foncée, fond gris clair
- **DragLeave** : Retour à l'état normal
- **Drop** : Traitement automatique du fichier
- **Click** : Ouverture du sélecteur de fichiers

### 📁 **Gestion des Fichiers**
```javascript
// État des fichiers
const [dragging, setDragging] = useState(false);
const [identityDragging, setIdentityDragging] = useState(false);

// Fonctions de traitement
const handleFile = (file: File | null) => onChange("cnpsProof", file);
const handleIdentityCard = (file: File | null) => onChange("identityCard", file);
```

### 🗑️ **Suppression de Fichiers**
- **Bouton X** : Apparaît quand fichier uploadé
- **Clic** : Remet le champ à null
- **Design** : Petit cercle blanc avec croix grise

---

## 🎨 **Design Responsive**

### 📱 **Mobile (< 640px)**
- **Boutons OUI/NON** : Stackés verticalement
- **Zones upload** : Largeur complète, espacement réduit
- **Textes** : Taille adaptée

### 💻 **Desktop (> 640px)**
- **Boutons OUI/NON** : Côte à côte
- **Zones upload** : Grille 1 colonne
- **Espacement** : Plus généreux

---

## 🚨 **Messages d'Erreur Spécifiques**

### ❌ **Si pas de sélection**
```javascript
"Veuillez sélectionner 'Oui' ou 'Non' pour la carte CNPS avant de continuer."
```

### ❌ **Si OUI mais pas de numéro**
```javascript
"Veuillez entrer votre numéro CNPS avant de continuer."
```

### ❌ **Si NON mais fichiers manquants**
```javascript
// Un seul fichier manquant
"Vous devez ajouter la preuve de paiement de 14500 FCFA pour permettre la création de votre carte CNPS avant de continuer."

// Deux fichiers manquants
"Vous devez ajouter votre pièce d'identité pour permettre la création de votre carte CNPS avant de continuer."
```

### 📋 **Affichage des Erreurs**
- **Modal** : Boîte de dialogue rouge
- **Titre** : "Documents manquants"
- **Messages** : Listes à puces rouges
- **Bouton** : "Compris" pour fermer

---

## 🔒 **Validation et Sécurité**

### ✅ **Validation Côté Client**
- **Types MIME** : Vérification formats acceptés
- **Taille fichiers** : Affichage en KB
- **Format numéro CNPS** : Pattern CI-AAAA-NNNNN

### 🛡️ **Sécurité des Données**
- **Compression** : Images compressées avant envoi
- **Base64 encoding** : Pas de stockage local
- **HTTPS obligatoire** : Transfert sécurisé

---

## 📊 **Résumé des Deux Cas**

| Caractéristique | CAS OUI (A CNPS) | CAS NON (Pas CNPS) |
|----------------|---------------------|----------------------|
| **Bouton actif** | OUI (fond noir) | NON (fond noir) |
| **Champs affichés** | 1 champ (numéro CNPS) | 2 zones upload |
| **Documents requis** | Numéro CNPS valide | Preuve paiement + Pièce identité |
| **Frais** | Aucun | 14 500 FCFA |
| **Validation** | Numéro obligatoire | 2 fichiers obligatoires |
| **Design** | Simple, minimaliste | Riche, informatif |
| **Instructions** | Aucune | Paiement WhatsApp/Wave |

---

## 🎯 **Objectif de l'Étape 3**

### ✅ **But Principal**
Permettre aux candidats sans CNPS de créer leur dossier en fournissant les justificatifs nécessaires, tout en gérant les deux cas de figure de manière claire et sécurisée.

### 🔄 **Flux Optimisé**
1. **Question simple** : OUI/NON pour la carte CNPS
2. **Adaptation** : Interface selon la réponse
3. **Guidage clair** : Instructions pour chaque cas
4. **Validation robuste** : Vérification avant progression
5. **Support intégré** : Coordonnées de paiement disponibles

---

*Étape 3 CNPS - Description complète et détaillée*
