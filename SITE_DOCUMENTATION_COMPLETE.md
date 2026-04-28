# Documentation Complète - Site de Recrutement Société Ivoirienne de Raffinage

## 📋 Vue d'ensemble

**Nom du projet** : Société Ivoirienne de Raffinage - Recrutement 2026  
**Type** : Formulaire de recrutement en ligne avec envoi d'emails  
**Technologies** : React + TypeScript + Vite + TailwindCSS + EmailJS  
**URL de production** : https://societe-ivoirienne-de-raffinage-git-main-sergio5091s-projects.vercel.app/  
**Repository** : https://github.com/Sergio5091/jo-2  

---

## 🏗️ Architecture Technique

### Stack Principal
- **Frontend** : React 18.3.1 avec TypeScript
- **Build Tool** : Vite 5.0.0
- **Styling** : TailwindCSS 3.4.0
- **Routing** : Wouter 3.3.5
- **Forms** : React Hook Form 7.55.0 avec Zod validation
- **UI Components** : Radix UI + Shadcn/ui
- **Email Service** : EmailJS Browser 4.4.1
- **Icons** : Lucide React 0.400.0
- **Animations** : Framer Motion 11.0.0

### Structure des Fichiers
```
src/
├── components/
│   ├── SuccessCard.tsx          # Page de confirmation
│   ├── ui/                     # Composants UI réutilisables
│   └── steps/
│       ├── StepIdentity.tsx       # Étape 1: Identité
│       ├── StepAvailability.tsx   # Étape 2: Disponibilité
│       ├── StepCNPS.tsx         # Étape 3: CNPS
│       └── StepEngagement.tsx    # Étape 4: Engagement
├── pages/
│   ├── Index.tsx                # Page d'accueil
│   ├── FormPage.tsx             # Formulaire principal
│   └── not-found.tsx           # Page 404
├── lib/utils.ts                # Utilitaires
├── main.tsx                   # Point d'entrée
└── index.css                  # Styles globaux
```

---

## 📧 Configuration EmailJS Complète

### 1. Variables d'Environnement

Créez un fichier `.env` à la racine du projet :

```env
# Email destinataire des formulaires
VITE_RECIPIENT_EMAIL=sergionounagnon1@gmail.com

# Configuration EmailJS
VITE_EMAILJS_SERVICE_ID=your_service_id
VITE_EMAILJS_TEMPLATE_ID=your_template_id
VITE_EMAILJS_PUBLIC_KEY=your_public_key
```

### 2. Configuration EmailJS Dashboard

#### Service Email
- **Type** : Gmail (ou autre provider)
- **Service ID** : À récupérer dans EmailJS

#### Template Email
- **Template ID** : À créer dans EmailJS
- **Sujet** : `Nouvelle candidature SIR - {{from_name}}`
- **Destinataire** : `sergionounagnon1@gmail.com`

#### Variables du Template
```javascript
{
  to_email: "sergionounagnon1@gmail.com",
  from_name: "Prénom Nom",
  employee_id: "SIR-03-123",
  full_name: "Nom de famille",
  first_name: "Prénom",
  phone: "+225 00 00 00 00",
  emergency_name: "Nom contact urgence",
  emergency_phone: "+225 00 00 00 00",
  health_issue: "Aucun problème de santé",
  availability_date: "2026-01-15",
  has_cnps: "Oui",
  cnps_number: "123456789",
  cnps_proof_name: "preuve_cnps.jpg",
  cnps_proof_base64: "data:image/jpeg;base64,...",
  identity_card_name: "carte_identite.jpg",
  identity_card_base64: "data:image/jpeg;base64,...",
  confirm_accuracy: "Oui",
  accept_terms: "Oui",
  submission_date: "17/04/2026 20:56:30"
}
```

### 3. Code EmailJS dans FormPage.tsx

```typescript
import emailjs from "@emailjs/browser";

const sendEmail = async () => {
  try {
    const serviceId = import.meta.env.VITE_EMAILJS_SERVICE_ID;
    const templateId = import.meta.env.VITE_EMAILJS_TEMPLATE_ID;
    const publicKey = import.meta.env.VITE_EMAILJS_PUBLIC_KEY;
    
    const emailData = {
      to_email: import.meta.env.VITE_RECIPIENT_EMAIL,
      from_name: `${formData.firstName} ${formData.fullName}`.trim(),
      employee_id: formData.employeeId,
      // ... autres variables
    };
    
    await emailjs.send(serviceId, templateId, emailData, publicKey);
    console.log("✅ Email envoyé avec succès !");
    return true;
  } catch (error) {
    console.error("Erreur EmailJS:", error);
    throw new Error("Erreur lors de l'envoi du formulaire.");
  }
};
```

---

## 📋 Flux du Formulaire

### Étape 1: Identité (StepIdentity.tsx)
- **ID Employé** : Format `SIR-03-XXX`
- **Nom complet** : Nom de famille
- **Prénom** : Prénom
- **Téléphone** : Format +225 XXXXXXXXX

### Étape 2: Disponibilité (StepAvailability.tsx)
- **Contact d'urgence** : Nom et téléphone
- **Problème de santé** : Optionnel, champ texte
- **Date de disponibilité** : Sélecteur de date

### Étape 3: CNPS (StepCNPS.tsx)
- **Avoir une carte CNPS** : Oui/Non
- **Numéro CNPS** : Si applicable
- **Preuve de paiement** : Upload d'image (compression automatique)
- **Carte d'identité** : Upload d'image (compression automatique)

### Étape 4: Engagement (StepEngagement.tsx)
- **Récapitulatif** : Affichage de toutes les données
- **Confirmation exactitude** : Checkbox obligatoire
- **Acceptation CGU** : Checkbox obligatoire
- **Soumission** : Envoi via EmailJS

---

## 🎨 Design et UX

### Composants UI
- **Boutons** : Style moderne avec hover effects
- **Formulaires** : Validation en temps réel
- **Alertes** : Toast notifications (Sonner)
- **Progression** : Barre de progression visuelle
- **Responsive** : Mobile-first design

### Styles
- **Couleurs principales** : Bleu professionnel (#1e40af)
- **Typography** : Inter font family
- **Spacing** : TailwindCSS spacing system
- **Animations** : Framer Motion transitions

---

## 🚀 Déploiement

### Vercel Configuration
```json
{
  "buildCommand": "npm run build",
  "installCommand": "npm install",
  "framework": "vite",
  "functions": {},
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/index.html"
    }
  ]
}
```

### Scripts Package.json
```json
{
  "scripts": {
    "dev": "PORT=5174 BASE_PATH=/ vite --config vite.config.ts --host 0.0.0.0",
    "build": "vite build --config vite.config.ts",
    "serve": "vite preview --config vite.config.ts --host 0.0.0.0",
    "typecheck": "tsc -p tsconfig.json --noEmit",
    "vercel-build": "vite build"
  }
}
```

---

## 🔧 Développement Local

### Installation
```bash
npm install
```

### Développement
```bash
npm run dev
# Accès : http://localhost:5174
```

### Build
```bash
npm run build
# Output : dist/
```

### Variables d'environnement locales
```bash
cp .env.example .env
# Éditer .env avec vos clés EmailJS
```

---

## 📊 Fonctionnalités Avancées

### Compression d'Images
- **Algorithme** : Canvas-based compression
- **Qualité** : 85% JPEG
- **Taille max** : ~500KB par image
- **Format** : Base64 pour EmailJS

### Validation de Formulaire
- **Schema Zod** : Validation côté client
- **Messages d'erreur** : En français
- **Validation en temps réel** : Pendant la saisie

### Gestion d'Erreurs
- **Try-catch** : Sur toutes les opérations async
- **Toast notifications** : Feedback utilisateur
- **Fallback values** : En cas d'erreur

---

## 🔒 Sécurité

### EmailJS
- **Clé publique** : Pas de clé privée exposée
- **Rate limiting** : Géré par EmailJS
- **Sanitization** : Variables échappées

### Upload de Fichiers
- **Compression** : Réduction de la taille
- **Validation** : Types MIME acceptés
- **Base64** : Pas de stockage local

---

## 📱 Performance

### Optimisations
- **Lazy loading** : Composants à la demande
- **Code splitting** : Vite automatic
- **Image optimization** : Compression automatique
- **Bundle size** : ~500KB gzippé

### Métriques cibles
- **FCP** : < 1.5s
- **LCP** : < 2.5s
- **FID** : < 100ms
- **CLS** : < 0.1

---

## 🔄 Maintenance

### Mises à jour
```bash
npm update
npm audit fix
```

### Logs
- **Console** : Logs détaillés EmailJS
- **Réseau** : Requêtes API visibles
- **Erreurs** : Stack traces complètes

### Monitoring
- **Vercel Analytics** : Traffic et performance
- **EmailJS Dashboard** : Statistiques d'envoi
- **Console errors** : Monitoring en temps réel

---

## 📞 Contact Support

### Pour les candidats
- **Email** : supportrecrut@gmail.com
- **WhatsApp/Wave** : 2250767554748

### Pour les développeurs
- **Repository** : https://github.com/Sergio5091/jo-2
- **Documentation** : Ce fichier
- **Issues** : GitHub issues

---

## 📝 Historique des Modifications

### v1.0.0 (17/04/2026)
- ✅ Changement de nom : "Port Autonome d'Abidjan" → "Société Ivoirienne de Raffinage"
- ✅ Configuration EmailJS complète
- ✅ Formulaire en 4 étapes
- ✅ Upload et compression d'images
- ✅ Déploiement Vercel configuré
- ✅ Design responsive et moderne
- ✅ Validation de formulaire robuste

---

## 🚀 Prochaines Évolutions

### Court terme (v1.1)
- [ ] Mode sombre
- [ ] Progression sauvegardée
- [ ] Export PDF des candidatures

### Moyen terme (v2.0)
- [ ] Dashboard admin
- [ ] Base de données candidates
- [ ] Notifications SMS

### Long terme (v3.0)
- [ ] Entretiens en ligne
- [ ] Signature électronique
- [ ] Intégration RH

---

*Document généré le 17/04/2026 - Société Ivoirienne de Raffinage*
