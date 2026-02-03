<div align="center">

# 🛡️ MetaSec Browser

### Navigateur Electron avec routage Tor et protection de la vie privée

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Electron](https://img.shields.io/badge/Electron-28.0.0-47848F?logo=electron)](https://www.electronjs.org/)
[![Tor](https://img.shields.io/badge/Tor-Integrated-7D4698?logo=tor-project)](https://www.torproject.org/)

<img src="https://github.com/user-attachments/assets/de170f2b-7374-46a3-9d55-f0ed8b006b27" alt="MetaSec Logo" width="200"/>

**Routage Tor • Bloqueur de Publicités • Isolation des Onglets • Interface Personnalisable**

[Installation](#-installation) • [Fonctionnalités](#-fonctionnalités-actuelles) • [Utilisation](#-utilisation) • [Prochaines Versions](#-roadmap)

</div>

---

## 📋 À Propos

MetaSec est un navigateur web basé sur Electron qui intègre le routage Tor via SOCKS5 et offre des fonctionnalités de protection de la vie privée. Conçu pour les utilisateurs soucieux de leur confidentialité en ligne, il combine une interface moderne avec des outils de sécurité essentiels.

### ⚠️ Important

MetaSec est un projet en développement actif. Il ne remplace pas le Tor Browser officiel pour un anonymat critique. Utilisez-le pour :
- Navigation privée quotidienne
- Apprentissage des technologies de confidentialité
- Développement et tests

**Pour anonymat critique : utilisez [Tor Browser](https://www.torproject.org/download/)**

---

## ✨ Fonctionnalités Actuelles

### 🔐 Sécurité & Confidentialité

- **Routage Tor Automatique** : Connexion via SOCKS5 (127.0.0.1:9050) avec Tor Expert Bundle intégré
- **Support des Ponts obfs4** : Contournement de la censure avec bridges Tor
- **Bloqueur de Publicités** : Filtres intégrés basés sur EasyList
- **Blocage des Trackers** : Protection contre le tracking tiers
- **Isolation des Sessions** : Chaque onglet utilise une partition de session isolée
- **Blocage WebRTC** : Prévention des fuites d'IP locale
- **Forçage HTTPS** : Redirection automatique HTTP → HTTPS
- **Blocage des Cookies Tiers** : Protection contre le tracking inter-sites
- **Permissions Bloquées** : Géolocalisation, caméra, micro bloqués par défaut

### 🌐 Navigation

- **Gestion Multi-Onglets** : Jusqu'à 9 onglets simultanés avec sauvegarde automatique
- **Historique de Navigation** : Boutons Précédent/Suivant fonctionnels par onglet
- **Moteurs de Recherche** : DuckDuckGo, Google, Bing, Yahoo avec support multilingue
- **Favoris** : Système de bookmarks avec sauvegarde locale
- **Menu Contextuel** : Clic droit avec options de navigation et recherche
- **Téléchargements** : Gestionnaire intégré avec barre de progression

### 🎨 Interface & Personnalisation

- **Thèmes** : Clair et Sombre (Kuja)
- **Page d'Accueil Personnalisable** : Fond d'écran custom, raccourcis rapides
- **Barre d'Outils** : Contrôles de fenêtre, indicateur Tor, boutons de navigation
- **Indicateur de Sécurité** : Affichage HTTPS/HTTP dans la barre d'adresse
- **Zoom de Page** : Contrôle du zoom (50% - 200%)
- **Taille de Police** : Ajustable (Petite, Moyenne, Grande, Très grande)

### ⚙️ Paramètres Avancés

- **Raccourcis Clavier** : Personnalisables (Ctrl+T, Ctrl+W, Ctrl+L, etc.)
- **User-Agent Personnalisé** : Configuration manuelle du User-Agent
- **Dossier de Téléchargement** : Sélection du répertoire de destination
- **Langues** : Support multilingue (FR, EN, ES, DE, IT, PT, RU, ZH, JA, AR, KO, NL, PL, TR)
- **Mode Volatile** : Sessions éphémères sans traces (option `--volatile`)
- **Profils Multiples** : Isolation complète par profil (option `--profile=nom`)

### 🛠️ Outils Intégrés

- **OSINT Toolbox** : Panneau latéral avec outils de recherche OSINT
- **Gestionnaire de Téléchargements** : Pause, reprise, annulation des téléchargements
- **DevTools** : Outils de développement Chromium accessibles (F12)
- **Notifications** : Système de notifications pour les actions importantes

---

## 🏗️ Architecture Technique

### Stack Technologique

- **Electron 28.0.0** : Framework principal
- **Chromium** : Moteur de rendu (sandboxé)
- **Tor Expert Bundle** : Routage anonyme
- **Node.js** : Backend et gestion des processus

### Configuration de Sécurité

```javascript
{
  contextIsolation: true,      // Isolation du contexte JavaScript
  sandbox: true,                // Sandbox Chromium activé
  nodeIntegration: false,       // Pas de Node.js dans le renderer
  enableRemoteModule: false,    // Module remote désactivé
  webSecurity: true,            // Sécurité web activée
  allowRunningInsecureContent: false
}
```

### Flags Chromium

- `--disable-webrtc` : Blocage WebRTC
- `--disable-background-networking` : Pas de requêtes en arrière-plan
- `--disable-breakpad` : Désactivation crash reporter
- `--disable-domain-reliability` : Pas de télémétrie

### Intégration Tor

```
┌──────────┐    SOCKS5     ┌─────┐    Encrypted    ┌──────────┐
│ MetaSec  │──────────────>│ Tor │───────────────> │ Internet │
│ Browser  │  127.0.0.1    │9050 │   3+ Hops       │          │
└──────────┘               └─────┘                 └──────────┘
```

**Configuration torrc :**
```
SocksPort 9050
ControlPort 9051
DataDirectory ./tor-data
Log notice stdout
IsolateDestAddr 1
IsolateDestPort 1
```

---

## 📦 Installation

### Prérequis

- **Node.js** ≥ 18.0.0
- **npm** ≥ 9.0.0
- **Windows** (support Linux/macOS en développement)

### Installation Rapide

```bash
# Cloner le repository
git clone https://github.com/votre-username/MetaSec.git
cd MetaSec

# Installer les dépendances
npm install

# Démarrer MetaSec
npm start
```

Le Tor Expert Bundle est inclus dans le projet et se lance automatiquement.

---

## 🚀 Utilisation

### Modes de Démarrage

```bash
# Mode standard (avec Tor)
npm start

# Mode volatile (session éphémère, aucune trace)
npm start -- --volatile

# Mode développement (DevTools activés)
npm run dev

# Profil isolé
npm start -- --profile=travail
```

### Raccourcis Clavier

| Raccourci | Action |
|-----------|--------|
| `Ctrl+T` | Nouvel onglet |
| `Ctrl+W` | Fermer l'onglet actif |
| `Ctrl+Shift+T` | Réouvrir le dernier onglet fermé |
| `Ctrl+L` | Focus sur la barre d'adresse |
| `Ctrl+D` | Ajouter aux favoris |
| `Ctrl+R` | Actualiser la page |
| `Ctrl+F` | Rechercher dans la page |
| `Ctrl+,` | Ouvrir les paramètres |
| `F12` | Ouvrir les DevTools |

### Vérifications de Sécurité

1. **Vérifier IP Tor :**
   - Visiter https://check.torproject.org
   - Message attendu : "Congratulations. This browser is configured to use Tor."

2. **Test fuites DNS :**
   - Visiter https://dnsleaktest.com
   - Aucun DNS de votre FAI ne doit apparaître

3. **Test WebRTC :**
   - Visiter https://browserleaks.com/webrtc
   - Aucune IP locale ne doit être exposée

---

## ⚙️ Configuration

### Paramètres Disponibles

**Confidentialité & Sécurité :**
- Suggestions de recherche
- Blocage des trackers (Basique, Équilibré, Agressif)
- Bloqueur de publicités
- DNS sécurisé (Cloudflare, Google, Quad9)
- Isolation des sites
- Cookies tiers

**Apparence :**
- Thème (Clair, Sombre)
- Affichage du bouton Accueil
- Affichage de la barre de favoris
- URLs complètes
- Taille de police
- Zoom de page

**Téléchargements :**
- Dossier de destination
- Ouverture automatique
- Demander l'emplacement

**Langues :**
- Interface multilingue
- Traduction automatique

### Fichiers de Configuration

```
~/.config/metasec/
├── profiles/
│   ├── default/          # Profil par défaut
│   └── travail/          # Profils personnalisés
├── user-agent-config.json
└── settings.json
```

---

## 🔧 Build et Distribution

### Formats de Distribution

- **Windows** : NSIS installer + Portable

---

## 📊 Roadmap

### 🚀 v0.2.0 - Système d'Identité par Onglet (Prochaine Version)

La prochaine mise à jour majeure introduira un système révolutionnaire de gestion d'identités par onglet :

**Fonctionnalités Prévues :**

- **Personas Multiples** : Créer et gérer plusieurs identités virtuelles
- **Isolation Complète** : Chaque persona avec ses propres cookies, cache, historique
- **Profils Contextuels** : Travail, Personnel, Recherche, Shopping, etc.
- **Rotation Automatique** : Changement d'identité automatique par onglet
- **User-Agent Dynamique** : User-Agent différent par persona
- **Indicateurs Visuels** : Couleurs et icônes pour identifier rapidement les personas
- **Sauvegarde des Sessions** : Restauration des onglets par persona
- **Circuits Tor Dédiés** : Circuit Tor isolé par identité

**Cas d'Usage :**
- Séparer vie professionnelle et personnelle
- Recherches OSINT avec identités multiples
- Tests de sites web avec différents profils
- Protection contre le tracking comportemental

### 🔮 Versions Futures

- **v0.3.0** : Support bridges Tor avancés, circuit isolation
- **v0.4.0** : Extensions NoScript/uBlock natives
- **v0.5.0** : Intégration I2P, multi-proxy
- **v0.6.0** : Fingerprinting résistance avancée
- **v1.0.0** : Audit sécurité externe, version stable

---

## 🤝 Contribution

Les contributions sont bienvenues ! Domaines prioritaires :

1. **Sécurité** : Audits, tests de fuites, hardening
2. **Fonctionnalités** : Nouvelles features, améliorations UI
3. **Documentation** : Guides, tutoriels, traductions
4. **Tests** : Couverture, tests E2E

### Workflow

```bash
# Fork et clone
git clone https://github.com/datadr1p/metasec.git

# Développer et tester
npm run
```

---

## � Licences

- **MetaSec** : [MIT License](LICENSE)
- **Electron** : MIT License
- **Chromium** : BSD License
- **Tor** : BSD 3-Clause License

---

## 🔒 Sécurité

Pour signaler une vulnérabilité de sécurité, veuillez créer une issue privée ou contacter directement les mainteneurs.

**Délai de réponse :** 48 heures

---

## 📞 Support

- **SimpleX Issues** : [Signaler un bug](https://smp8.simplex.im/g#YKN3ug-nfu2YDnLF0A39URtxLfBPrUDCpY03rtEppww)

---

## 🙏 Remerciements

- **Tor Project** : Pour le réseau Tor et la documentation
- **Electron Team** : Pour le framework
- **EasyList** : Pour les filtres de blocage de publicités
- **Communauté Open Source** : Pour les contributions et le feedback

---

## ⚖️ Avertissement Légal

MetaSec est un outil de protection de la vie privée. Les utilisateurs sont responsables de l'utilisation qu'ils en font. Le projet ne cautionne aucune activité illégale.

---

<div align="center">

**Construit avec ❤️ pour la privacy**

⭐ **Si MetaSec vous est utile, n'hésitez pas à star le projet !** ⭐

[⬆ Retour en haut](#-metasec-browser)

</div>
