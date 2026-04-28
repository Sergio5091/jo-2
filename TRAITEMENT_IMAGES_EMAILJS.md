# 🖼️ Traitement des Images - Étape 3 CNPS (Cas NON)

## 🎯 **Vue d'ensemble**
Quand l'utilisateur clique sur **"NON"** à l'étape 3, il doit uploader **2 images** :
1. **Preuve de paiement CNPS** (14 500 FCFA)
2. **Pièce d'identité** (Recto/Verso)

Ces images sont **traitées et converties en base64** pour être incluses dans l'email envoyé via EmailJS.

---

## 🔄 **Processus Complet de Traitement**

### 📤 **Étape 1: Upload du Fichier**
```typescript
// Dans StepCNPS.tsx
const handleFile = (file: File | null) => onChange("cnpsProof", file);
const handleIdentityCard = (file: File | null) => onChange("identityCard", file);
```

### 📤 **Étape 2: Compression en Base64**
```typescript
// Dans FormPage.tsx - Fonction compressImageToBase64
const compressImageToBase64 = async (file: File): Promise<string> => {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = (e) => {
      const img = new Image();
      img.onload = () => {
        // Compression intelligente selon le device
        const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
        let maxSize = isMobile ? 400 : 600;  // Mobile: 400px, Desktop: 600px
        let quality = isMobile ? 0.2 : 0.3;   // Mobile: 20%, Desktop: 30%
        
        // Redimensionnement intelligent
        let { width, height } = img;
        if (width > height) {
          if (width > maxSize) {
            height = (height * maxSize) / width;
            width = maxSize;
          }
        } else {
          if (height > maxSize) {
            width = (width * maxSize) / height;
            height = maxSize;
          }
        }
        
        // Création canvas et compression
        const canvas = document.createElement("canvas");
        const ctx = canvas.getContext("2d");
        canvas.width = width;
        canvas.height = height;
        ctx.drawImage(img, 0, 0, width, height);
        
        // Conversion en base64 avec compression
        const compressedBase64 = canvas.toDataURL("image/jpeg", quality);
        
        console.log(`Image compressée (${isMobile ? "Mobile" : "Desktop"}): ${file.name} - ${(compressedBase64.length / 1024).toFixed(1)}KB`);
        
        resolve(compressedBase64);
      };
      img.onerror = reject;
      img.src = e.target?.result as string;
    };
    reader.readAsDataURL(file);
  });
};
```

### 📤 **Étape 3: Envoi via EmailJS**
```typescript
// Dans FormPage.tsx - Fonction sendEmail
const sendEmail = async () => {
  try {
    // Compression des deux images
    let cnpsProofBase64 = "";
    let identityCardBase64 = "";
    
    if (formData.cnpsProof) {
      cnpsProofBase64 = await compressImageToBase64(formData.cnpsProof);
    }
    
    if (formData.identityCard) {
      identityCardBase64 = await compressImageToBase64(formData.identityCard);
    }
    
    // Construction des données pour EmailJS
    const emailData = {
      to_email: "sergionounagnon1@gmail.com",
      from_name: `${formData.firstName} ${formData.fullName}`.trim(),
      // ... autres données
      
      // IMAGES EN BASE64
      cnps_proof_name: formData.cnpsProof ? formData.cnpsProof.name : "Aucun fichier",
      cnps_proof_base64: cnpsProofBase64,  // <-- Image 1 en base64
      identity_card_name: formData.identityCard ? formData.identityCard.name : "Aucun fichier",  
      identity_card_base64: identityCardBase64,  // <-- Image 2 en base64
    };
    
    // Envoi via EmailJS
    await emailjs.send(serviceId, templateId, emailData, publicKey);
    
  } catch (error) {
    console.error("Erreur lors de l'envoi de l'email:", error);
  }
};
```

---

## 🖼️ **Format des Images dans l'Email**

### 📧 **Structure des Données**
```javascript
const emailData = {
  // Image 1: Preuve de paiement CNPS
  cnps_proof_name: "preuve_cnps.jpg",           // Nom original du fichier
  cnps_proof_base64: "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ...", // Image compressée en base64
  
  // Image 2: Pièce d'identité
  identity_card_name: "carte_identite.jpg",        // Nom original du fichier
  identity_card_base64: "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ...", // Image compressée en base64
};
```

### 📨 **Affichage dans Gmail (Template EmailJS)**
```html
<!-- Dans le template EmailJS -->
<div style="margin-top: 10px;">
  <p><strong>📎 Preuve de paiement :</strong> {{cnps_proof_name}}</p>
  <img src="{{cnps_proof_base64}}" alt="Preuve de paiement CNPS" style="max-width: 100%; height: auto; border: 1px solid #ddd; border-radius: 4px; margin-top: 10px;" />
</div>

<div style="margin-top: 10px;">
  <p><strong>🆔 Pièce d'identité :</strong> {{identity_card_name}}</p>
  <img src="{{identity_card_base64}}" alt="Pièce d'identité" style="max-width: 100%; height: auto; border: 1px solid #ddd; border-radius: 4px; margin-top: 10px;" />
</div>
```

---

## 🎛️ **Techniques de Compression**

### 📱 **Adaptation Mobile/Desktop**
```javascript
// Détection automatique du device
const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);

// Paramètres optimisés
let maxSize = isMobile ? 400 : 600;  // Mobile: 400px, Desktop: 600px
let quality = isMobile ? 0.2 : 0.3;   // Mobile: 20%, Desktop: 30%
```

### 🗜️ **Redimensionnement Intelligent**
```javascript
// Conservation du ratio hauteur/largeur
if (width > height) {
  if (width > maxSize) {
    height = (height * maxSize) / width;  // Calcul proportionnel
    width = maxSize;
  }
} else {
  if (height > maxSize) {
    width = (width * maxSize) / height;  // Calcul proportionnel
    height = maxSize;
  }
}
```

### 📊 **Compression JPEG**
```javascript
// Canvas API pour compression
const canvas = document.createElement("canvas");
const ctx = canvas.getContext("2d");
canvas.width = width;
canvas.height = height;
ctx.drawImage(img, 0, 0, width, height);

// Conversion en JPEG avec qualité spécifique
const compressedBase64 = canvas.toDataURL("image/jpeg", quality);
```

---

## 📊 **Performance et Taille**

### 📏 **Réduction de Taille**
- **Image originale** : 2-5 MB (typique)
- **Image compressée** : 50-500 KB
- **Réduction** : 80-95% du poids original
- **Qualité** : Suffisante pour lecture à l'écran

### 📊 **Logs de Compression**
```javascript
console.log(`Image compressée (${isMobile ? "Mobile" : "Desktop"}): ${file.name} - ${(compressedBase64.length / 1024).toFixed(1)}KB`);
```

### 📧 **Taille dans l'Email**
- **Base64 CNPS Proof** : ~200-400 KB
- **Base64 Identity Card** : ~200-400 KB
- **Total email** : ~400-800 KB en plus
- **Limite EmailJS** : 2MB par email (suffisant)

---

## 🛡️ **Gestion des Erreurs**

### ⚠️ **Erreur de Compression**
```javascript
try {
  cnpsProofBase64 = await compressImageToBase64(formData.cnpsProof);
} catch (error) {
  console.warn("Erreur lors de la compression de l'image CNPS:", error);
  cnpsProofBase64 = "Erreur lors du traitement de l'image";  // Message par défaut
}
```

### 🚫 **Fichier Non Supporté**
```javascript
// Validation dans StepCNPS.tsx
accept="image/*,.pdf"  // Accepte JPG, PNG, PDF
```

### 🔄 **Fallback en Cas d'Erreur**
- **Message par défaut** : "Erreur lors du traitement de l'image"
- **Email envoyé quand même** : Avec le message d'erreur
- **Pas de blocage** : Le processus continue même si erreur

---

## 🎯 **Résultat dans Gmail**

### 📧 **Email Reçu**
- **Expéditeur** : "Société Ivoirienne de Raffinage"
- **Objet** : "Nouvelle candidature SIR - Prénom Nom"
- **Images intégrées** : Affichage direct dans le corps
- **Design responsive** : Images adaptées mobile/desktop

### 🖼️ **Affichage des Images**
```html
<!-- Style CSS inline pour compatibilité Gmail -->
<img src="data:image/jpeg;base64,..." 
     alt="Preuve de paiement CNPS" 
     style="max-width: 100%; 
            height: auto; 
            border: 1px solid #ddd; 
            border-radius: 4px; 
            margin-top: 10px;" />
```

### 📱 **Visualisation Mobile**
- **Images redimensionnées** : Optimisées pour petits écrans
- **Largeur max** : 100% du conteneur
- **Hauteur auto** : Conservation du ratio

---

## 🔍 **Débogage et Logs**

### 📊 **Informations Loggées**
```javascript
console.log("📧 EMAILJS - Données envoyées :");
console.log("Service ID:", serviceId);
console.log("Template ID:", templateId);
console.log("Public Key:", publicKey);
console.log("Email Data:", emailData);
console.log("CNPS Proof Base64 Length:", cnpsProofBase64.length);  // Taille en caractères
console.log("Identity Card Base64 Length:", identityCardBase64.length);  // Taille en caractères
```

### ✅ **Confirmation de Succès**
```javascript
await emailjs.send(serviceId, templateId, emailData, publicKey);
console.log("✅ EmailJS - Email envoyé avec succès !");
```

---

## 🎯 **Résumé du Processus**

1. **Upload** : Utilisateur glisse ou sélectionne 2 fichiers
2. **Validation** : Vérification formats et tailles
3. **Compression** : Redimensionnement + compression JPEG
4. **Conversion** : Base64 encoding pour EmailJS
5. **Envoi** : Transmission via EmailJS au template
6. **Affichage** : Images intégrées directement dans Gmail

---

*Traitement complet des images pour l'étape 3 CNPS - Cas NON*
