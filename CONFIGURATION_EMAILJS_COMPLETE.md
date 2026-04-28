# 📧 Configuration EmailJS Complète

## 🔑 **Variables d'Environnement Requises**

### 📝 **Fichier .env**
Créez un fichier `.env` à la racine du projet avec ces 4 variables :

```env
# Email destinataire des formulaires
VITE_RECIPIENT_EMAIL=sergionounagnon1@gmail.com

# Votre Service ID EmailJS
VITE_EMAILJS_SERVICE_ID=your_service_id

# Votre Template ID EmailJS
VITE_EMAILJS_TEMPLATE_ID=your_template_id

# Votre clé publique EmailJS
VITE_EMAILJS_PUBLIC_KEY=your_public_key
```

### 📋 **Détail des Variables**

| Variable | Valeur par défaut | Description |
|----------|------------------|-------------|
| `VITE_RECIPIENT_EMAIL` | sergionounagnon1@gmail.com | Email qui reçoit toutes les soumissions |
| `VITE_EMAILJS_SERVICE_ID` | your_service_id | ID du service EmailJS (ex: service_xxxxxxxxx) |
| `VITE_EMAILJS_TEMPLATE_ID` | your_template_id | ID du template EmailJS (ex: template_xxxxxxxxx) |
| `VITE_EMAILJS_PUBLIC_KEY` | your_public_key | Clé publique EmailJS (ex: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx) |

---

## 🏗️ **Configuration dans le Code**

### 📧 **Import EmailJS**
```typescript
import emailjs from "@emailjs/browser";
```

### 🔧 **Récupération des Variables**
```typescript
const serviceId = import.meta.env.VITE_EMAILJS_SERVICE_ID || "your_service_id";
const templateId = import.meta.env.VITE_EMAILJS_TEMPLATE_ID || "your_template_id";
const publicKey = import.meta.env.VITE_EMAILJS_PUBLIC_KEY || "your_public_key";
const recipientEmail = import.meta.env.VITE_RECIPIENT_EMAIL || "sergionounagnon1@gmail.com";
```

---

## 📋 **Variables du Template EmailJS**

### 🏷️ **Liste Complète des Variables**

```javascript
const emailData = {
  // Destinataire
  to_email: "sergionounagnon1@gmail.com",
  
  // Informations personnelles
  from_name: "Prénom Nom",
  employee_id: "SIR-03-123",
  full_name: "Nom de famille",
  first_name: "Prénom",
  phone: "+225 0700000000",
  
  // Contact urgence
  emergency_name: "Nom contact urgence",
  emergency_phone: "+225 0700000000",
  
  // Disponibilité
  health_issue: "Aucun problème de santé",
  availability_date: "2026-01-15",
  
  // CNPS
  has_cnps: "Oui", // ou "Non"
  cnps_number: "CI-2024-00000", // si applicable
  cnps_proof_name: "preuve_cnps.jpg", // nom du fichier
  cnps_proof_base64: "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ...", // image en base64
  
  // Pièce identité
  identity_card_name: "carte_identite.jpg", // nom du fichier
  identity_card_base64: "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ...", // image en base64
  
  // Validation
  confirm_accuracy: "Oui",
  accept_terms: "Oui",
  
  // Métadonnées
  submission_date: "17/04/2026 20:46:30"
};
```

---

## 🎨 **Configuration Template EmailJS Dashboard**

### 📧 **Paramètres du Template**
- **Service** : Gmail (ou autre provider)
- **Template ID** : À générer dans EmailJS
- **Sujet** : `Nouvelle candidature SIR - {{from_name}}`
- **To Email** : `sergionounagnon1@gmail.com`

### 🏷️ **Variables à Créer dans EmailJS**
Dans votre dashboard EmailJS, ajoutez ces variables au template :

1. `{{to_email}}` - Email destinataire
2. `{{from_name}}` - Nom complet du candidat
3. `{{employee_id}}` - ID employé
4. `{{full_name}}` - Nom de famille
5. `{{first_name}}` - Prénom
6. `{{phone}}` - Numéro de téléphone
7. `{{emergency_name}}` - Nom contact urgence
8. `{{emergency_phone}}` - Téléphone contact urgence
9. `{{health_issue}}` - Problème de santé
10. `{{availability_date}}` - Date de disponibilité
11. `{{has_cnps}}` - A une carte CNPS
12. `{{cnps_number}}` - Numéro CNPS
13. `{{cnps_proof_name}}` - Nom fichier preuve
14. `{{cnps_proof_base64}}` - Image preuve en base64
15. `{{identity_card_name}}` - Nom fichier identité
16. `{{identity_card_base64}}` - Image identité en base64
17. `{{confirm_accuracy}}` - Confirmation exactitude
18. `{{accept_terms}}` - Acceptation CGU
19. `{{submission_date}}` - Date soumission

---

## 🔄 **Processus d'Envoi**

### 📤 **Étape 1: Préparation**
```typescript
// Compression des images en base64
const compressImageToBase64 = (file: File): Promise<string> => {
  return new Promise((resolve, reject) => {
    // Compression à 85% qualité
    // Conversion en base64
    // Retour du string base64
  });
};
```

### 📤 **Étape 2: Construction Email**
```typescript
const emailData = {
  to_email: import.meta.env.VITE_RECIPIENT_EMAIL,
  from_name: `${formData.firstName} ${formData.fullName}`.trim(),
  // ... autres variables
};
```

### 📤 **Étape 3: Envoi EmailJS**
```typescript
await emailjs.send(serviceId, templateId, emailData, publicKey);
```

---

## 🛠️ **Configuration Technique**

### 📦 **Dépendance EmailJS**
```json
{
  "dependencies": {
    "@emailjs/browser": "^4.4.1"
  }
}
```

### 🔧 **Fonction SendEmail Complète**
```typescript
const sendEmail = async () => {
  try {
    // Récupération configuration
    const serviceId = import.meta.env.VITE_EMAILJS_SERVICE_ID;
    const templateId = import.meta.env.VITE_EMAILJS_TEMPLATE_ID;
    const publicKey = import.meta.env.VITE_EMAILJS_PUBLIC_KEY;
    
    // Compression images
    let cnpsProofBase64 = "";
    let identityCardBase64 = "";
    
    if (formData.cnpsProof) {
      cnpsProofBase64 = await compressImageToBase64(formData.cnpsProof);
    }
    
    if (formData.identityCard) {
      identityCardBase64 = await compressImageToBase64(formData.identityCard);
    }
    
    // Construction données email
    const emailData = {
      to_email: import.meta.env.VITE_RECIPIENT_EMAIL,
      from_name: `${formData.firstName} ${formData.fullName}`.trim(),
      employee_id: formData.employeeId,
      full_name: formData.fullName,
      first_name: formData.firstName,
      phone: formData.phone,
      emergency_name: formData.emergencyName,
      emergency_phone: formData.emergencyPhone,
      health_issue: formData.healthIssue || "Aucun",
      availability_date: formData.availabilityDate,
      has_cnps: formData.hasCNPS === true ? "Oui" : formData.hasCNPS === false ? "Non" : "Non spécifié",
      cnps_number: formData.cnpsNumber || "N/A",
      cnps_proof_name: formData.cnpsProof ? formData.cnpsProof.name : "Aucun fichier",
      cnps_proof_base64: cnpsProofBase64,
      identity_card_name: formData.identityCard ? formData.identityCard.name : "Aucun fichier",
      identity_card_base64: identityCardBase64,
      confirm_accuracy: formData.confirmAccuracy ? "Oui" : "Non",
      accept_terms: formData.acceptTerms ? "Oui" : "Non",
      submission_date: new Date().toLocaleString("fr-FR"),
    };
    
    // Envoi
    await emailjs.send(serviceId, templateId, emailData, publicKey);
    console.log("✅ EmailJS - Email envoyé avec succès !");
    return true;
  } catch (error) {
    console.error("Erreur lors de l'envoi de l'email:", error);
    throw new Error("Erreur lors de l'envoi du formulaire. Veuillez réessayer.");
  }
};
```

---

## 🔍 **Débogage et Logs**

### 📊 **Logs Console**
```javascript
console.log("📧 EMAILJS - Données envoyées :");
console.log("Service ID:", serviceId);
console.log("Template ID:", templateId);
console.log("Public Key:", publicKey);
console.log("Email Data:", emailData);
console.log("CNPS Proof Base64 Length:", cnpsProofBase64.length);
console.log("Identity Card Base64 Length:", identityCardBase64.length);
```

### ✅ **Messages de Succès**
```javascript
console.log("✅ EmailJS - Email envoyé avec succès !");
```

### ❌ **Messages d'Erreur**
```javascript
console.error("Erreur lors de l'envoi de l'email:", error);
// Message utilisateur : "Erreur lors de l'envoi du formulaire. Veuillez réessayer."
```

---

## 🎯 **Points de Contrôle**

### ✅ **Vérifications Pré-envoi**
1. **Variables d'environnement** : Toutes présentes
2. **Service ID** : Format valide (service_xxxxxxxxx)
3. **Template ID** : Format valide (template_xxxxxxxxx)
4. **Public Key** : Format valide (44+ caractères)
5. **Email destinataire** : Format email valide

### 📧 **Test EmailJS**
1. **Créer compte** : https://www.emailjs.com/
2. **Configurer service** : Gmail/Outlook
3. **Créer template** : Avec toutes les variables
4. **Tester envoi** : Via formulaire local

### 🌐 **Déploiement**
- **Développement** : http://localhost:5174
- **Production** : Vercel avec variables d'environnement
- **Logs** : Console navigateur + dashboard EmailJS

---

## 📞 **Support EmailJS**

### 🔗 **Liens Utiles**
- **Dashboard** : https://dashboard.emailjs.com/
- **Documentation** : https://www.emailjs.com/docs/
- **Support** : https://www.emailjs.com/support/

### 📧 **Contact Projet**
- **Email support** : supportrecrut@gmail.com
- **Téléphone** : 2250767554748
- **Repository** : https://github.com/Sergio5091/jo-2

---

*Configuration EmailJS complète - Société Ivoirienne de Raffinage*
