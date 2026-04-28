# 📋 Design et Comportement Détaillé des Pages du Site

## 🏠 Page d'Accueil (Index.tsx)

### 🎨 **Design Visuel**
- **Header fixe** : Fond blanc avec bordure grise, logo SIR à gauche
- **Logo** : Format 24x9px, alt="Société Ivoirienne de Raffinage"
- **Titre** : "Société Ivoirienne de Raffinage" en uppercase tracking-widest
- **Sous-titre** : "Direction des Ressources Humaines · République de Côte d'Ivoire"
- **Badge** : "Recrutement 2026" (caché sur mobile)
- **Background** : Blanc pur (#FFFFFF)
- **Couleurs** : Gris professionnel (#1F2937, #6B7280, #9CA3AF)

### 📝 **Texte Principal**
```
Félicitations, vous êtes présélectionné(e) !

Vous avez été retenu(e) pour poursuivre le processus de recrutement au 
Société Ivoirienne de Raffinage.
Veuillez compléter votre dossier en remplissant le formulaire ci-dessous.
```

### 🎯 **Comportement Interactif**
- **Animation fade-in-up** : Apparition progressive du contenu
- **Bouton CTA** : "Accéder au formulaire" avec icône PenLine
- **Hover effects** : Bouton gris (#111827) au survol
- **Navigation** : Redirection vers `/formulaire` au clic

### 📊 **Cartes d'Information**
1. **Stage professionnel**
   - Icône Briefcase
   - Texte : "Durée de 3 à 6 mois"
   
2. **Inscription rapide**
   - Icône Clock
   - Texte : "Formulaire en 4 étapes"
   
3. **Données sécurisées**
   - Icône Shield
   - Texte : "Informations strictement confidentielles"

### 📱 **Responsive Design**
- **Mobile** : Cartes en 1 colonne
- **Desktop** : Cartes en grille 3 colonnes
- **Header** : Logo + titre stacké sur mobile

---

## 📝 Page Formulaire (FormPage.tsx)

### 🎨 **Design Global**
- **Header fixe** : Logo + titre + sous-titre "Dossier de candidature — Recrutement 2026"
- **Stepper visuel** : Barre de progression avec 4 étapes
- **Formulaire** : Carte blanche avec ombre subtile
- **Footer** : Contact support

### 📊 **Stepper de Progression**
```
Étape 1: Identité (icône User)
Étape 2: Disponibilité (icône Calendar)  
Étape 3: CNPS (icône CreditCard)
Étape 4: Engagement (icône CheckSquare)
```
- **Barre de progression** : Gris avec progression en noir
- **Animations** : Transitions fluides 500ms
- **États** : Actif (ring-2), Terminé (fond gris), À venir (gris clair)

### 🔄 **Navigation Étapes**

#### Boutons Navigation
- **Précédent** : Bordure grise, désactivé sur première étape
- **Suivant** : Fond gris #111827, icône ChevronRight
- **Soumettre** : Visible uniquement à l'étape 4, icône CheckSquare

#### Modal d'Erreurs
- **Design** : Boîte rouge avec icône "!" 
- **Overlay** : Fond noir semi-transparent
- **Animations** : Fade-in avec backdrop blur
- **Messages** : Listes d'erreurs en rouge sur fond rouge clair

### 📋 **Étape 1: Identité (StepIdentity.tsx)**
**Champs obligatoires :**
- ID Employé : Format "SIR-03-XXX"
- Nom complet : Champ texte
- Prénom : Champ texte  
- Téléphone : Préfixé "+225 "

**Validation :**
- Tous les champs obligatoires
- Format téléphone valide
- Modal d'erreur si champs manquants

### 📅 **Étape 2: Disponibilité (StepAvailability.tsx)**
**Champs :**
- Contact urgence : Nom + téléphone
- Problème santé : Optionnel, textarea
- Date disponibilité : Sélecteur de date

**Validation :**
- Date obligatoire
- Téléphone urgence format valide

### 💳 **Étape 3: CNPS (StepCNPS.tsx)**
**Logique conditionnelle :**
- **Si OUI** : Numéro CNPS obligatoire
- **Si NON** : Preuve paiement + carte identité obligatoires

**Upload de fichiers :**
- Compression automatique des images
- Prévisualisation avant upload
- Base64 encoding pour EmailJS

### ✅ **Étape 4: Engagement (StepEngagement.tsx)**
**Récapitulatif complet :**
- Toutes les données saisies
- Format structuré avec labels
- Images intégrées si présentes

**Checkboxes obligatoires :**
- Confirmation exactitude des informations
- Acceptation conditions générales

### 📧 **Soumission et Envoi**
**Processus EmailJS :**
1. Compression images en base64
2. Construction objet emailData
3. Appel `emailjs.send()`
4. Loading spinner pendant envoi
5. Redirection vers page succès

**Gestion erreurs :**
- Toast notifications
- Messages d'erreur détaillés
- Retry possible

---

## ✅ Page de Succès (SuccessCard.tsx)

### 🎨 **Design Visuel**
- **Header** : Logo + titre "Société Ivoirienne de Raffinage"
- **Background** : Gris clair (#F9FAFB)
- **Cercle vert** : Icône CheckCircle dans fond vert
- **Animation** : fade-in-up

### 📝 **Texte de Confirmation**
```
Dossier soumis avec succès

Votre candidature a bien été enregistrée. Vous serez contacté(e) par la
Direction des Ressources Humaines dans les plus brefs délais.
```

### 📞 **Coordonnées Display**
- **Email** : supportrecrut@gmail.com avec icône Mail
- **Téléphone** : "WhatsApp / Wave : 2250767554748" avec icône Phone
- **Design** : Carte blanche avec bordure grise

### 🎯 **Comportement**
- **Bouton "Retour à l'accueil"** : Redirection vers page principale
- **Hover effects** : Bouton gris foncé au survol
- **Responsive** : Centré sur tous les écrans

---

## ❌ Page 404 (not-found.tsx)

### 🎨 **Design Minimaliste**
- **Background** : Gris clair (#F9FAFB)
- **Carte** : Fond blanc avec ombre
- **Icône** : AlertCircle en rouge
- **Titre** : "404 Page Not Found"

### 📝 **Message**
```
Did you forget to add the page to the router?
```

### 🎯 **Comportement**
- **Centrage vertical/horizontal** : Flexbox center
- **Responsive** : Max-width sur mobile
- **Pas de navigation** : Page d'erreur simple

---

## 🎨 **Système de Design Global**

### 🎯 **Palette de Couleurs**
- **Primaire** : Gris foncé (#111827)
- **Secondaire** : Gris moyen (#6B7280)
- **Success** : Vert (#059669)
- **Erreur** : Rouge (#DC2626)
- **Warning** : Orange (#D97706)
- **Background** : Blanc (#FFFFFF)
- **Surface** : Gris clair (#F9FAFB)

### 📝 **Typography**
- **Police** : Inter (Google Fonts)
- **Titres** : Font-semibold, tracking-widest
- **Textes** : Font-normal
- **Tailles** : xs (12px), sm (14px), base (16px), xl (20px)

### 🎭 **Animations**
- **Fade-in-up** : Apparition depuis le bas
- **Transitions** : 300ms duration
- **Hover states** : Changement de couleur
- **Loading** : Spin animation sur SVG

### 📱 **Responsive Breakpoints**
- **Mobile** : < 640px (sm)
- **Tablet** : 640px - 1024px
- **Desktop** : > 1024px

### 🧩 **Composants UI**
- **Boutons** : Arrondis (rounded-md), ombre subtile
- **Formulaires** : Bordures grises, focus ring bleu
- **Cartes** : Fond blanc, bordures grises
- **Modaux** : Overlay flou, animations fluides

---

## 🔄 **Flux Utilisateur Complet**

### 1. **Arrivée sur Page d'Accueil**
- Message de félicitations
- Information sur le processus
- CTA vers formulaire

### 2. **Navigation Formulaire**
- Stepper visuel clair
- Navigation avant/arrière
- Validation à chaque étape

### 3. **Soumission**
- Loading pendant envoi
- Confirmation immédiate
- Envoi EmailJS en arrière-plan

### 4. **Page de Succès**
- Confirmation visuelle
- Coordonnées support
- Retour possible à l'accueil

### 5. **Gestion Erreurs**
- Modaux informatifs
- Messages clairs
- Navigation bloquée si requis

---

## 🔒 **Sécurité et Validation**

### ✅ **Validations Frontend**
- Champs obligatoires
- Formats téléphone/email
- Taille de fichiers limitée
- Types MIME acceptés

### 🛡️ **Protection Données**
- Pas de stockage local
- Compression images côté client
- Envoi sécurisé via EmailJS
- HTTPS obligatoire

---

*Documentation générée le 17/04/2026 - Design complet du site SIR*
