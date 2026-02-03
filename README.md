<div align="center">

# 🛡️ MetaSec Browser

### Navigateur Electron durci pour l'OPSEC et la confidentialité

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Electron](https://img.shields.io/badge/Electron-28.0.0-47848F?logo=electron)](https://www.electronjs.org/)
[![Security](https://img.shields.io/badge/Security-Hardened-green.svg)]()
[![Tor](https://img.shields.io/badge/Tor-Integrated-7D4698?logo=tor-project)](https://www.torproject.org/)

<img src="https://github.com/user-attachments/assets/de170f2b-7374-46a3-9d55-f0ed8b006b27" alt="MetaSec Logo" width="200"/>

**Privacy-First • Tor Native • Zero Telemetry • Session Isolation**

[Installation](#-installation) • [Documentation](#-documentation) • [Sécurité](#-architecture-de-sécurité) • [Tests](#-tests-de-sécurité)

</div>

---

## ⚠️ Avertissement de Sécurité

> **MetaSec est un prototype éducatif et ne peut PAS remplacer le Tor Browser officiel pour un anonymat critique.**

**Limitations connues :**
- Empreinte navigateur différente du Tor Browser Bundle
- Protection partielle contre le fingerprinting avancé (canvas, WebGL, fonts)
- Surface d'attaque Electron plus large que Firefox ESR durci
- Pas de protection NoScript/HTTPS Everywhere intégrée

**Pour anonymat critique : utilisez [Tor Browser](https://www.torproject.org/download/) + [Tails](https://tails.boum.org/) ou [QubesOS](https://www.qubes-os.org/)**

---

## 🎯 Objectifs du Projet

MetaSec est un navigateur web basé sur Electron conçu pour les professionnels de la cybersécurité, chercheurs OSINT, et utilisateurs soucieux de leur vie privée. Il intègre nativement le routage Tor et implémente des mécanismes de défense contre le tracking, le fingerprinting et les fuites réseau.

### Principes Fondamentaux

- **Zero Trust** : Aucune télémétrie, aucun tracking, aucune donnée envoyée à des tiers
- **Privacy by Design** : Isolation réseau par défaut, sessions volatiles, DNS via proxy
- **Defense in Depth** : Multiples couches de protection (sandbox, CSP, headers sécurisés)
- **Transparency** : Code source ouvert, auditable, sans obfuscation

---

## ✨ Fonctionnalités Principales

### 🔐 Sécurité & Privacy

| Fonctionnalité | Description |
|----------------|-------------|
| **Routage Tor Natif** | Intégration SOCKS5 avec Tor Expert Bundle, circuits isolés |
| **Session Isolation** | Chaque onglet utilise une partition de session isolée (cookies, cache, storage) |
| **Mode Volatile** | Sessions éphémères stockées en RAM/tmp, supprimées automatiquement |
| **Anti-Fingerprinting** | Randomisation canvas, masquage WebGL, normalisation timezone UTC |
| **Zero DNS Leak** | Résolution DNS forcée via proxy SOCKS5, pas de requêtes directes |
| **WebRTC Disabled** | Blocage complet de WebRTC pour éviter les fuites d'IP locale |
| **CSP Stricte** | Content Security Policy durcie, headers de sécurité renforcés |
| **Sandbox Chromium** | Isolation complète du renderer process, pas d'accès Node.js |

### 🛠️ Fonctionnalités Avancées

- **Profils Multiples** : Isolation complète par profil (work, personal, research)
- **Conteneurs d'Onglets** : Groupes d'onglets avec isolation réseau dédiée
- **OSINT Toolbox** : Outils intégrés pour recherche OSINT (Shodan, Censys, etc.)
- **Extensions Whitelistées** : Support d'extensions auditées (uBlock Origin, HTTPS Everywhere)
- **Bookmarks Manager** : Gestion avancée des favoris avec tags et recherche
- **Thèmes Personnalisables** : Interface adaptable (clair, sombre, glassmorphism)

---

## 🏗️ Architecture de Sécurité

### Modèle de Processus

```
┌─────────────────────────────────────────────────────────┐
│                    Main Process                          │
│  • Gestion fenêtres et sessions                         │
│  • Configuration proxy Tor (SOCKS5)                     │
│  • Isolation réseau par conteneur                       │
│  • IPC handlers sécurisés                               │
└────────────┬────────────────────────────────────────────┘
             │ IPC via contextBridge (whitelist strict)
             │
┌────────────▼────────────────────────────────────────────┐
│                  Preload Script                          │
│  • Bridge sécurisé entre main et renderer               │
│  • Validation et sanitization des entrées               │
│  • API minimale exposée (principe du moindre privilège) │
└────────────┬────────────────────────────────────────────┘
             │ contextBridge API
             │
┌────────────▼────────────────────────────────────────────┐
│                 Renderer Process                         │
│  • Sandboxed Chromium (pas d'accès Node.js)            │
│  • UI navigateur et gestion onglets                     │
│  • Restrictions CSP strictes                            │
└─────────────────────────────────────────────────────────┘
```

### Configuration Electron Durcie

```javascript
{
  contextIsolation: true,           // Isolation contexte JS
  sandbox: true,                     // Sandbox Chromium activé
  nodeIntegration: false,            // Pas de Node.js dans renderer
  enableRemoteModule: false,         // Module remote désactivé
  webSecurity: true,                 // Web security activée
  allowRunningInsecureContent: false // Blocage contenu mixte
}
```

### Flags Chromium de Sécurité

- `--disable-webrtc` : Blocage WebRTC
- `--disable-background-networking` : Pas de requêtes en arrière-plan
- `--disable-breakpad` : Désactivation crash reporter
- `--disable-domain-reliability` : Pas de télémétrie domaine
- `--no-pings` : Blocage hyperlink auditing
- `--disable-remote-fonts` : Pas de fonts externes

### Intégration Tor

```
┌──────────┐    SOCKS5     ┌─────┐    Encrypted    ┌──────────┐
│ MetaSec  │──────────────>│ Tor │───────────────>│ Internet │
│ Browser  │  127.0.0.1    │9050 │   3+ Hops      │          │
└──────────┘               └─────┘                 └──────────┘
```

**Configuration torrc :**
```
SocksPort 9050
ControlPort 9051
DataDirectory ~/.metasec/tor-data
Log notice stdout
StrictNodes 1
ExitNodes {us},{ca},{de},{nl}
```

---

## 📦 Installation

### Prérequis

- **Node.js** ≥ 18.0.0
- **npm** ≥ 9.0.0
- **Tor Expert Bundle** (téléchargé automatiquement)

### Installation Rapide

```bash
# Cloner le repository
git clone https://github.com/votre-org/metasec.git
cd metasec

# Installer les dépendances (Tor téléchargé automatiquement)
npm install

# Démarrer MetaSec
npm start
```

### Installation Manuelle de Tor (optionnel)

Si le téléchargement automatique échoue :

**Linux/macOS :**
```bash
wget https://dist.torproject.org/torbrowser/13.0/tor-expert-bundle-linux-x86_64-13.0.tar.gz
tar -xzf tor-expert-bundle-*.tar.gz -C tor/
```

**Windows :**
```powershell
# Télécharger depuis https://www.torproject.org/download/tor/
# Extraire dans tor/
```

---

## 🚀 Utilisation

### Modes de Démarrage

```bash
# Mode standard (avec Tor)
npm start

# Mode volatile (session éphémère)
npm run start:volatile

# Mode développement (DevTools activés)
npm run dev

# Profil isolé
npm start -- --profile=work
```

### Vérifications Post-Installation

1. **Vérifier IP Tor :**
   - Visiter https://check.torproject.org
   - Message attendu : "Congratulations. This browser is configured to use Tor."

2. **Test fuites DNS :**
   ```bash
   npm run test:dns-leak
   ```
   Ou visiter https://dnsleaktest.com

3. **Test WebRTC :**
   - Visiter https://browserleaks.com/webrtc
   - Aucune IP locale ne doit être exposée

---

## 🧪 Tests de Sécurité

### Tests Automatisés

```bash
# Suite complète de tests
npm test

# Tests de sécurité uniquement
npm run test:security

# Tests fuites DNS
npm run test:dns-leak

# Tests end-to-end
npm run test:e2e

# Audit dépendances
npm run audit
```

### Tests Manuels

| Test | URL | Résultat Attendu |
|------|-----|------------------|
| **IP Tor** | https://check.torproject.org | IP = Tor exit node |
| **DNS Leak** | https://dnsleaktest.com | Pas de DNS FAI |
| **WebRTC Leak** | https://browserleaks.com/webrtc | Pas d'IP locale |
| **Fingerprinting** | https://coveryourtracks.eff.org | Score élevé |
| **IPv6 Leak** | https://test-ipv6.com | IPv6 non détecté |

---

## 🔧 Configuration Avancée

### Profils Personnalisés

```bash
# Créer un profil dédié
npm start -- --profile=osint

# Structure des profils
~/.config/metasec/profiles/
├── default/
│   ├── Cookies
│   ├── Local Storage/
│   └── Session Storage/
└── osint/
    └── ...
```

### Configuration Tor Personnalisée

Éditer `tor-expert-bundle/tor/torrc` :

```
# Utiliser des bridges (contourner censure)
UseBridges 1
Bridge obfs4 [IP]:[PORT] [FINGERPRINT]

# Forcer pays de sortie
ExitNodes {ch},{is},{se}
StrictNodes 1

# Augmenter circuits
NumEntryGuards 8
```

### Extensions Whitelistées

```javascript
// Dans src/main.js
const extensions = [
  '/path/to/ublock-origin',
  '/path/to/https-everywhere'
];

extensions.forEach(ext => {
  session.defaultSession.loadExtension(ext, {
    allowFileAccess: false
  });
});
```

---

## 📚 Documentation

- **[Architecture Technique](docs/ARCHITECTURE.md)** : Spécifications détaillées, modèle de menaces
- **[Guide de Sécurité](docs/SECURITY.md)** : Checklist audit, tests de fuites, hardening
- **[Guide Utilisateur](docs/USER_GUIDE.md)** : Tutoriels, cas d'usage, best practices
- **[Modèle de Menaces](docs/THREAT_MODEL.md)** : Analyse des risques, vecteurs d'attaque
- **[Roadmap](docs/ROADMAP.md)** : Fonctionnalités futures, améliorations prévues

---

## 🛡️ Comparaison avec Tor Browser

| Critère | MetaSec | Tor Browser |
|---------|---------|-------------|
| **Routage Tor** | ✅ SOCKS5 | ✅ Natif |
| **Isolation Circuits** | ❌ | ✅ |
| **Anti-Fingerprinting** | 🟡 Partiel | ✅ Complet |
| **NoScript Intégré** | ❌ | ✅ |
| **HTTPS Everywhere** | ❌ | ✅ |
| **Empreinte Uniforme** | ❌ | ✅ |
| **Mises à Jour Tor** | 🟡 Manuelle | ✅ Auto |
| **Surface d'Attaque** | 🔴 Electron | 🟢 Firefox ESR |
| **Extensibilité** | ✅ | ❌ |
| **OSINT Tools** | ✅ | ❌ |

**Verdict :** MetaSec pour recherche/développement, Tor Browser pour anonymat critique.

---

## 🚨 Limitations et Risques Résiduels

### Vecteurs d'Attaque Non Mitigés

1. **Fingerprinting Avancé**
   - Fonts système exposées
   - Audio context fingerprinting
   - Canvas fingerprinting (randomisé mais détectable)

2. **Timing Attacks**
   - Pas de protection contre corrélation temporelle
   - Latence réseau analysable

3. **Exploits Chromium**
   - Surface d'attaque plus large que Firefox ESR
   - Dépendance aux patches de sécurité Electron

4. **Corrélation Comportementale**
   - Patterns d'utilisation analysables
   - Pas de protection contre l'analyse de trafic avancée

### Recommandations de Mitigation

- **Utiliser dans une VM** : QubesOS, VirtualBox avec snapshots
- **VPN + Tor** : Cacher l'usage de Tor au FAI (VPN → Tor → Internet)
- **Sessions Courtes** : Limiter la durée des sessions pour réduire corrélation
- **Pas d'Identifiants Réels** : Ne jamais se connecter avec identité personnelle
- **Mises à Jour Régulières** : Maintenir Electron et Tor à jour

---

## 🏗️ Build et Distribution

### Build Multi-Plateformes

```bash
# Toutes les plateformes
npm run build

# Spécifique
npm run build:win      # Windows (NSIS + Portable)
npm run build:mac      # macOS (DMG + notarization)
npm run build:linux    # Linux (AppImage + deb)
```

### Signature des Binaires

**Windows (Authenticode) :**
```bash
export CSC_LINK=/path/to/cert.pfx
export CSC_KEY_PASSWORD=your_password
npm run build:win
```

**macOS (Notarization) :**
```bash
export APPLE_ID=your@email.com
export APPLE_ID_PASSWORD=app-specific-password
npm run build:mac
```

**Linux (GPG) :**
```bash
gpg --detach-sign --armor dist/MetaSec-*.AppImage
```

### Vérification Checksums

```bash
# Générer checksums
sha256sum dist/* > SHA256SUMS

# Signer
gpg --clearsign SHA256SUMS

# Vérifier
sha256sum -c SHA256SUMS
```

---

## 🤝 Contribution

Les contributions sont bienvenues ! Priorités :

1. **Sécurité** : Audits, tests de fuites, hardening
2. **Privacy** : Anti-fingerprinting, anti-tracking
3. **Documentation** : Guides, tutoriels, traductions
4. **Tests** : Couverture, tests E2E, fuzzing

### Workflow

```bash
# Fork et clone
git clone https://github.com/votre-username/metasec.git

# Créer branche
git checkout -b feature/ma-fonctionnalite

# Développer et tester
npm test
npm run lint

# Commit et push
git commit -m "feat: description"
git push origin feature/ma-fonctionnalite

# Créer Pull Request
```

### Guidelines

- Code style : ESLint + Prettier
- Commits : [Conventional Commits](https://www.conventionalcommits.org/)
- Tests : Couverture ≥ 80%
- Documentation : Mise à jour obligatoire

---

## 📜 Licences

- **MetaSec** : [MIT License](LICENSE)
- **Electron** : MIT License
- **Chromium** : BSD License
- **Tor** : BSD 3-Clause License

Aucun code propriétaire ou reverse-engineered.

---

## 🔒 Reporting de Vulnérabilités

**Email sécurisé :** security@metasec.local

**PGP Key :** `[À publier]`

**Délai de réponse :** 48 heures

**Responsible Disclosure :** 90 jours après patch

### Format de Report

```
Titre: [SECURITY] Description courte
Sévérité: Critical/High/Medium/Low
Composant: main.js / preload.js / tor / etc.
Description: Détails techniques complets
Reproduction: Steps to reproduce
Impact: Conséquences potentielles
Mitigation: Suggestions de fix (optionnel)
```

---

## 📞 Support et Communauté

- **GitHub Issues** : [Issues](https://github.com/votre-org/metasec/issues)
- **Discussions** : [GitHub Discussions](https://github.com/votre-org/metasec/discussions)
- **Matrix** : `#metasec:matrix.org`
- **Reddit** : [r/privacy](https://reddit.com/r/privacy)

---

## 🙏 Remerciements

- **Tor Project** : Pour le réseau Tor et la documentation
- **Electron Team** : Pour le framework et les guides de sécurité
- **EFF** : Pour les outils de test (Cover Your Tracks)
- **OWASP** : Pour les guidelines de sécurité web

---

## 📊 Roadmap

- [ ] **v0.2.0** : Support bridges Tor, circuit isolation
- [ ] **v0.3.0** : Intégration I2P, multi-proxy
- [ ] **v0.4.0** : Extensions NoScript/uBlock natives
- [ ] **v0.5.0** : Fingerprinting résistance avancée
- [ ] **v1.0.0** : Audit sécurité externe, certification

Voir [ROADMAP.md](docs/ROADMAP.md) pour détails.

---

<div align="center">

**Construit avec ❤️ pour la privacy • Audité avec 🔍 pour la sécurité**

⭐ **Si MetaSec vous est utile, n'hésitez pas à star le projet !** ⭐

[⬆ Retour en haut](#-metasec-browser)

</div>
