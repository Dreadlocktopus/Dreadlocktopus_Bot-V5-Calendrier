# Dreadlocktopus_Bot-V5-Calendrier
Bot de Programmation pour les diffusions de la communauté BigscreenFR.fr enrichi de fonctionnalités sociales.

<!-- BANNER -->
<div align="center">

```
🦑🐙  ᙃɾᥱᥲᑯꙆoᥴƙtoρᥙ⳽  🐙🦑
```

# Dreadlocktopus_Bot V5

**Bot Discord dédié à la communauté BigscreenFR.fr**  
Programmation de séances · Fun & Jeux · Calendrier en ligne · Administration complète

![Discord](https://img.shields.io/badge/Discord-Bot-5865F2?style=for-the-badge&logo=discord&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![GitHub Pages](https://img.shields.io/badge/GitHub_Pages-Calendrier-222222?style=for-the-badge&logo=github&logoColor=white)

</div>

---

## 📋 Sommaire

| Section | Description |
|---|---|
| [⚙️ Installation & Configuration](#-installation--configuration) | Mise en place du bot |
| [🎭 Hiérarchie des rôles](#-hiérarchie-des-rôles) | Niveaux d'accès |
| [📅 Programmation de séances](#-programmation-de-séances) | `/programmer` — Créer une diffusion |
| [🎬 Gestion des diffusions](#-gestion-des-diffusions) | `/gestion` — Modifier, annuler, programmes |
| [📡 Publication des séances](#-publication-des-séances) | `/diffusions` & `/seances` |
| [🌐 Calendrier en ligne](#-calendrier-en-ligne) | Site GitHub Pages auto-généré |
| [🔔 Rappels automatiques](#-rappels-automatiques) | DM avant les séances |
| [✉️ DM automatiques](#-dm-automatiques) | Confirmations & rappels diffuseur |
| [🎉 Fun & Social](#-fun--social) | `/fun` — Tous les jeux et défis |
| [🎮 Défis & Jeux](#-défis--jeux) | Synopsis · Note mystère · Qui est-ce ? · Trouve ton film |
| [🎬 Quiz & Citations](#-quiz--citations) | Quiz IA · Kaamelott · Citations Cinéma · Aventure |
| [🎭 Réactions & GIFs](#-réactions--gifs) | Suspense · Bravos · Mindfuck · Navet · Popcorn |
| [🗳️ Sondages](#-sondages) | Vote · Duel |
| [💖 Équipe](#-équipe) | Statik · Personnages IA |
| [🎲 Pitch IA](#-pitch-ia) | `/pitch` — Découverte de films |
| [🔗 Liens utiles](#-liens-utiles) | `/liensutiles` |
| [🛡️ Administration](#-administration) | `/admin` · `/reset` · `/sauvegarde` · `/restaurer` |
| [👑 SuperAdmin](#-superadmin) | `/superadmin` — Panneau de contrôle Owner |
| [📊 Statistiques](#-statistiques) | Données du serveur |
| [🔧 Configuration technique](#-configuration-technique) | `config.py` — Toutes les variables |

---

## ⚙️ Installation & Configuration

### Prérequis

- Python **3.11+**
- Un bot Discord créé sur le [Discord Developer Portal](https://discord.com/developers/applications)
- Un compte [TMDB](https://www.themoviedb.org/settings/api) (gratuit) pour les métadonnées films
- Un compte [OMDb](https://www.omdbapi.com/apikey.aspx) (gratuit) pour les titres alternatifs
- Un dépôt GitHub pour le calendrier en ligne (GitHub Pages)
- Une clé [Klipy](https://klipy.com/developers) ou [Giphy](https://developers.giphy.com/) pour les GIFs

### Installation

```bash
# 1. Cloner ou copier le projet
cd votre-dossier

# 2. Installer les dépendances
pip install discord.py pytz babel aiohttp

# 3. Remplir config.py (voir section Configuration)

# 4. Lancer le bot
python bot.py
```

### Configuration — `config.py`

Ouvrez `config.py` et renseignez toutes les valeurs :

```python
# ── Discord ───────────────────────────────────────────────────────
BOT_TOKEN = "votre_token_discord"         # Discord Developer Portal → Bot → Token
OWNER_ID  = 123456789012345678            # Votre ID Discord (clic droit → Copier l'ID)

# Salons Discord (ID numérique)
SEANCES_CHANNEL_ID    = 0  # Salon où les séances seront publiées (#séances-à-venir)
CALENDRIER_CHANNEL_ID = 0  # Salon du calendrier mensuel (#calendrier)

# ── Rôles ─────────────────────────────────────────────────────────
ADMINBOT_ROLE         = "AdminBot"                          # Nom exact du rôle Admin bot
ROLES_PROGRAMMATION   = ["Diffuseur", "Administrateur", "AdminBot"]  # Rôles pouvant programmer

# ── APIs Cinéma ───────────────────────────────────────────────────
TMDB_API_KEY  = "votre_cle_tmdb"          # themoviedb.org
OMDB_API_KEY  = "votre_cle_omdb"          # omdbapi.com
TMDB_LANGUAGE = "fr-FR"                   # Langue des métadonnées

# ── GitHub Pages (Calendrier en ligne) ───────────────────────────
GITHUB_TOKEN    = "ghp_votre_token"       # github.com/settings/tokens (scope: repo)
GITHUB_USERNAME = "VotreNomGitHub"        # Votre nom d'utilisateur GitHub
GITHUB_REPO     = "Nom-Du-Depot"          # ⚠️ PAS D'ESPACE dans le nom !

# ── GIFs ──────────────────────────────────────────────────────────
KLIPY_API_KEY = "votre_cle_klipy"         # klipy.com/developers
GIPHY_API_KEY = "votre_cle_giphy"         # developers.giphy.com (secours)

# ── Limites de programmation ──────────────────────────────────────
LIMITE_JOURS_DIFFUSEUR = 28               # Jours max à l'avance pour un Diffuseur
LIMITE_JOURS_ADMINBOT  = 1825             # Jours max pour AdminBot / Owner (5 ans)

# ── Serveur ───────────────────────────────────────────────────────
NOM_SERVEUR = "Nom de votre serveur"      # Affiché sur le site calendrier
```

> ⚠️ **Important :** Le nom du dépôt GitHub (`GITHUB_REPO`) ne doit **pas contenir d'espace**. Utilisez des tirets : `Mon-Calendrier` ✅ — `Mon Calendrier` ❌

---

## 🎭 Hiérarchie des rôles

Le bot reconnaît automatiquement le niveau de chaque membre selon ses rôles Discord. Aucun menu spécial à configurer — le bot détecte tout seul.

| Niveau | Rôle Discord | Accès |
|--------|-------------|-------|
| 👑 **Owner** | ID défini dans `config.py` | Tout — sans restriction |
| 🛡️ **AdminBot** | Rôle `AdminBot` (configurable) | Administration complète + programmation étendue |
| ⚙️ **Administrateur** | Rôle `Administrateur` ou permissions admin | Gestion complète des séances |
| 🎬 **Diffuseur** | Rôle `Diffuseur` | Programmer et gérer ses propres diffusions |
| 👤 **Spectateur** | Tous les autres membres | Consulter, s'inscrire aux séances, /fun |

> 💡 **L'aide s'adapte automatiquement** : `/aide` affiche uniquement les commandes accessibles à chaque membre selon son rôle.

---

## 📅 Programmation de séances

### `/programmer`

> 🔒 **Accès :** Diffuseur · Administrateur · AdminBot · Owner

Ouvre un formulaire guidé en plusieurs étapes pour créer et publier une nouvelle séance de cinéma.

#### Étape 1 — Type de contenu

Choisissez dans le menu déroulant :

| Option | Description |
|--------|-------------|
| 🎬 Film | Long-métrage |
| 📺 Série | Épisodes ou saison |
| 🎌 Animé | Animation japonaise / asiatique |
| 🎥 Documentaire | Film documentaire |
| 🎵 Concert | Captation de concert |
| 🎭 Spectacle | One-man-show, pièce de théâtre |

#### Étape 2 — Recherche du titre

- Le bot interroge automatiquement les bases **TMDB**, **OMDb** et **AniList**
- Fautes de frappe tolérées, accents optionnels
- Pour les animés : vous pouvez coller directement le titre en japonais, coréen ou chinois
- Pour les concerts et spectacles introuvables : saisie libre acceptée

#### Étape 3 — Sélection de l'affiche

- Naviguez parmi les résultats avec les boutons **◀** et **▶**
- Choisissez le format de projection :
  - **📽️ 2D** — projection standard
  - **🥽 3D** — projection en relief

#### Contenu adulte (+16 ans)

Si le contenu est classifié +16 ans, une alerte s'affiche automatiquement.

> ☑️ **La case `Je confirme — salle NSFW` est obligatoire.** Sans cocher cette case, la programmation est impossible, sans exception. Le nom de la salle Bigscreen doit également contenir `[NSFW]`.

#### Étape 4 — Date & Heure

Formats acceptés (heure France) :

```
Date     → 2102   ou   21/02          (jour et mois)
         → 01052025 ou 01/05/2025     (avec année — AdminBot/Owner uniquement)
Heure    → 2100   ou   21:00
```

> 🛡️ **AdminBot & Owner** peuvent programmer jusqu'à **5 ans** à l'avance et **dans le passé** (pour archiver d'anciennes diffusions).

#### Étape 5 — Nom de la salle Bigscreen

Entrez le nom **exact** de votre salle telle qu'elle est configurée dans l'application Bigscreen VR.

#### Étape 6 — Pseudo Bigscreen

- Votre pseudo Bigscreen est identique à Discord → laissez **vide**
- Votre pseudo est différent → saisissez-le ici

#### Résultat

✅ La séance est publiée dans le salon `#séances-à-venir` avec :
- Affiche du film/série
- Horaires France 🇫🇷 et Québec 🍁
- Format 2D ou 3D
- Nom de la salle et pseudo du diffuseur
- Durée, classification âge, note TMDB
- Lien vers la bande-annonce
- Bouton **« Je viens »** pour s'inscrire
- Boutons d'ajout au calendrier (Google Agenda & Apple/iCal)

---

## 🎬 Gestion des diffusions

### `/gestion`

> 🔒 **Accès :** Tous les rôles (fonctionnalités variables selon le niveau)

Ouvre le panneau de gestion interactif. La réponse est **éphémère** (visible uniquement par vous).

#### 📋 Mes diffusions

Affiche toutes vos séances (passées et à venir). Cliquez sur une séance pour accéder à ses options :

- **✏️ Modifier** — Changer la date, l'heure, la salle ou le pseudo Bigscreen
- **🗑️ Annuler** — Supprimer la séance (confirmation obligatoire)

> Lorsqu'une séance est modifiée ou annulée, **tous les membres inscrits via « Je viens » reçoivent automatiquement un DM** pour les informer.

#### 📺 Toutes les diffusions

> 🔒 Administrateur · AdminBot · Owner uniquement

Vue complète de toutes les séances de tous les diffuseurs. Permet de modifier ou annuler n'importe quelle séance.

#### 🗓️ Programme compact

> 🔒 Administrateur · AdminBot · Owner uniquement

Affiche les **10 prochaines diffusions** (paramétrable jusqu'à 25) dans un format condensé :

- Horaires France 🇫🇷 et Québec 🍁
- Salle Bigscreen et diffuseur
- Note TMDB ⭐
- Durée ⏱️ et classification âge
- Lien TMDB 🎬 et lien bande-annonce ▶

#### 🖼️ Programme riche

> 🔒 Administrateur · AdminBot · Owner uniquement

Cartes visuelles navigables (10 par défaut, max 25) avec :

- Affiche du film/série
- Résumé complet
- Durée ⏱️ et classification âge
- Lien TMDB et lien bande-annonce

#### 🛠️ Panneau Admin

Accès rapide au panneau d'administration complet (voir section [Administration](#-administration)).

---

## 📡 Publication des séances

### `/diffusions [nombre]`

> 🔒 **Accès :** Administrateur · AdminBot · Owner

Publie des **cartes visuelles** des séances à venir dans le salon — **visible par tous les membres**.

```
/diffusions       → toutes les séances à venir
/diffusions 5     → les 5 prochaines séances
```

Chaque carte affiche :
- Affiche du film/série avec lien TMDB
- Horaires France 🇫🇷 et Québec 🍁
- Salle Bigscreen · Diffuseur · Format 2D/3D
- Durée ⏱️ · Classification âge 🔞
- Lien bande-annonce ▶

> 💡 Idéal pour les annonces visuelles dans les salons publics. Partageable directement !

### `/seances`

> 🔒 **Accès :** Administrateur · AdminBot · Owner

Publie une **liste compacte** de toutes les séances à venir + lien vers le calendrier en ligne.

---

## 🌐 Calendrier en ligne

Le bot génère et publie automatiquement un **site web statique** sur GitHub Pages répertoriant toutes les diffusions à venir.

### Fonctionnement automatique

- **Mise à jour à chaque nouvelle séance** programmée
- **Mise à jour à chaque modification** ou annulation
- **Mise à jour quotidienne** automatique à minuit
- **Mise à jour au démarrage** du bot

### Contenu du site

- Calendrier mensuel interactif
- Fiches détaillées par séance (affiche, résumé, horaires, salle, bandes annonces, lien de la fiche du film)
- Liens d'ajout au calendrier personnel (Google, Apple, iCal)
- Page 404 personnalisée

### Mise à jour manuelle

Via `/superadmin → Outils N0 → 📅 Calendrier`, le Owner peut forcer une mise à jour immédiate avec deux options :
- **🏠 Ce serveur** — Refresh instantané ✅
- **🌐 Global (~1h)** — Propagation Discord lente

### Calendrier Discord

Le bot maintient également un **salon Discord dédié** (`#calendrier`) avec un calendrier mensuel mis à jour automatiquement, affichant toutes les séances mois par mois.

---

## 🔔 Rappels automatiques

> 🔒 **Configuration :** AdminBot · Owner (via `/superadmin`)

Le système de rappels envoie automatiquement un **DM aux membres inscrits** (via le bouton « Je viens ») avant chaque séance.

### Configuration des paliers

Depuis `/superadmin → 📢 Rappels & DM` :

```
Paliers par défaut : 60 minutes · 30 minutes · 15 minutes
→ DM envoyé à chacun de ces délais avant le début de la séance
```

Vous pouvez définir autant de paliers que vous le souhaitez :

```
Exemple : 120 60 30 15
→ DM à 2h, 1h, 30min et 15min avant la séance
```

### Statut

| Commande | Action |
|----------|--------|
| `Activer` | Active les rappels automatiques |
| `Désactiver` | Désactive (la configuration est conservée) |
| `Paliers` | Modifie les délais de rappel |

---

## ✉️ DM automatiques

> 🔒 **Configuration :** AdminBot · Owner (via `/superadmin`)

Le bot peut envoyer des **messages directs automatiques** dans deux situations :

### DM de confirmation

Envoyé au diffuseur juste après avoir programmé une séance. Confirme que la séance a bien été enregistrée avec tous ses détails.

### DM de rappel diffuseur

Envoyé directement au diffuseur avant SA propre séance (selon les paliers configurés).

---

## 🎉 Fun & Social

### `/fun`

> 🔒 **Accès :** Tous les membres (fonctionnalités variables selon le niveau)

Tapez `/fun` dans n'importe quel salon pour ouvrir le **panneau interactif** regroupant tous les jeux, défis, réactions et sondages.

---

## 🎮 Défis & Jeux

### 📽️ Synopsis foireux

> **Accès :** Tous les membres

Un synopsis **volontairement catastrophique** d'un film de votre choix, révélé en **5 actes progressifs**.

**Comment jouer :**
1. Sélectionnez `📽️ Synopsis foireux` dans le menu `/fun → Défis`
2. Saisissez le titre d'un film ou laissez blanc, 
3. Choisissez optionnellement une catégorie et un genre
4. L'IA génère un synopsis foireux dévoilé acte par acte

---

### ⭐ Note mystère

> **Accès :** Tous les membres

Une note s'affiche à l'écran. **Vraie ou inventée ?** Les membres votent — la révélation arrive à la fin !

**Comment jouer :**
1. Sélectionnez `⭐ Note mystère` dans le menu `/fun → Défis`
2. Une note (ex. `7.4/10`) apparaît pour un film mystère
3. Les membres votent : 🟢 Vraie note · 🔴 Note inventée
4. Le verdict tombe à la fin du vote

---

### 🎭 Qui est-ce ?

> **Accès :** Tous les membres

Des **indices progressifs** sur un personnage de film ou série. Le premier membre à taper le bon nom dans le chat gagne !

**Comment jouer :**
1. Sélectionnez `🎭 Qui est-ce ?` dans le menu `/fun → Défis`
2. Les indices se dévoilent un par un
3. Tapez votre réponse directement dans le chat
4. Premier arrivé, premier servi !

---

### 🔎 Trouve ton film

> **Accès :** Tous les membres

**5 indices TMDB** révélés un par un. Devinette collective dans le salon !

**Comment jouer :**
1. Sélectionnez `🔎 Trouve ton film` dans le menu `/fun → Défis`
2. Naviguez entre les indices avec les boutons **◀** et **▶**
3. Discutez et trouvez le film avec les autres membres !

---

### 🌀 Cross-Univers

> 🔒 **Accès :** Owner uniquement

Génère une **collision absurde entre deux univers cinématographiques** via l'IA : l'histoire du film A racontée dans l'univers du film B.

**Exemple :** *Et si Titanic se passait dans l'univers de Mad Max ?*

---

## 🎬 Quiz & Citations

### 🤖 Quiz IA

> **Accès :** Tous les membres (configurable)

**QCM multi-sources** généré par intelligence artificielle. Choisissez votre catégorie et votre niveau de difficulté.

**Catégories disponibles :**
- 🎬 Cinéma · 🎵 Musique · 📚 Culture générale · 🏆 Sport · 🍽️ Gastronomie · Et plus...

**Paramétrage (Owner/AdminBot via `/superadmin`) :**
- Nombre de rounds (par défaut : 5)
- Temps de réponse par round
- Pause entre les rounds
- Niveau minimum requis

---

### 💬 Citation Kaamelott

> **Accès :** Configurable par l'Owner

Retrouvez **qui a dit cette réplique** de la série Kaamelott ! QCM à 4 choix parmi les personnages.

**Comment jouer :**
1. Une réplique culte de Kaamelott s'affiche
2. Choisissez parmi 4 personnages
3. Bonne ou mauvaise réponse — c'est pas faux ! 🏰

---

### 🎬 Citation Cinéma

> **Accès :** Configurable par l'Owner

Retrouvez **le film ou le personnage** à partir d'une réplique de cinéma culte. QCM à 4 choix.

---

### ⚔️ Aventure Kaamelott

> **Accès :** Tous les membres

**Scénario textuel interactif** dans l'univers d'Alexandre Astier, piloté par l'intelligence artificielle.

Chaque décision vous emmène vers un destin différent. L'IA incarne les personnages et fait évoluer l'histoire selon vos choix.

---

### 🍔 Burger Quiz

> 🔒 **Accès :** Owner uniquement

Un quiz délirant en **3 rounds**, inspiré du célèbre jeu TV, arbitré par une IA absurde.

| Round | Nom | Format |
|-------|-----|--------|
| 1 | 🍗 Nuggets | QCM classique |
| 2 | 🧂 Sel ou Poivre | Vrai / Faux |
| 3 | 🍔 Burger de la Mort | 10 questions à mémoriser + 10 secondes chrono |

---

### 🐉 Kamoulox

> 🔒 **Accès :** Owner uniquement

**Duel absurde** joueur contre le bot alimenté par l'IA avec le célèbre arbitre **John-Bob**, l'arbitre IA au raisonnement impénétrable. Tour par tour, au fil d'affrontements surréalistes.

---

## 🎭 Réactions & GIFs

### Réactions

> **Accès :** Tous les membres

Depuis `/fun → Réactions` :

| Réaction | Effet |
|----------|-------|
| 😰 **Suspense** | GIF de suspense intense |
| 👏 **Bravos** | GIF d'applaudissements |
| 🤯 **Mindfuck** | GIF d'explosion mentale |
| 🥦 **Navet** | GIF pour les films ratés |

---

### 🍿 GIFs Popcorn

> 🔒 **Accès :** Administrateur · AdminBot · Owner

Envoie un GIF de popcorn dans le salon depuis `/fun → GIFs`.

---

### ⚔️ GIFs Kaamelott

> 🔒 **Accès :** Liste restreinte (élite des semi-croustillants) (reconnaissance via Discord_ID)

GIFs thématiques de la série Kaamelott. L'accès est limité à une liste d'utilisateurs définie par l'Owner.

> 🏰 Si vous faites partie de l'élite, le bouton apparaît dans votre menu `/fun → GIFs`.

Envoie un GIF de Kaamelott dans le salon depuis `/fun → GIFs` avec une citation aléatoire issue d'un API Kaamelott.

### ⚔️ GIFs Les Nuls

> 🔒 **Accès :** Administrateur · AdminBot · Owner

Envoie un GIF de popcorn dans le salon depuis `/fun → GIFs`.

---

## 🗳️ Sondages

### 🗳️ Vote sur un film

> **Accès :** Tous les membres

Depuis `/fun → Sondages → Vote` :

1. Saisissez le titre d'un film
2. La jaquette TMDB s'affiche automatiquement
3. Les membres votent : 👍 Oui · 👎 Non · 🤷 Peu importe
4. Les résultats s'affichent en temps réel

---

### ⚔️ Duel entre deux films

> **Accès :** Tous les membres

Depuis `/fun → Sondages → Duel` :

1. Saisissez les titres de deux films
2. Les jaquettes s'affichent côte à côte
3. Vote ouvert pendant **60 secondes**
4. Le vainqueur est proclamé à l'issue du duel

---

## 💖 Équipe

### 💖 Statik

> **Accès :** Tous les membres

Génère un **message de bienveillance, d'amour et d'encouragement** aléatoire parmi une banque de **300 phrases**. Ce message glorifie, complimente, est bienveillant envers le membre Dreadlocktopus et le Dreadlocktopus_Bot.

Depuis `/fun → Équipe → Statik` ou directement via `/statik`.

---

### 🌙 Personnages IA — 

> 🔒 **Accès :** Owner (accès élargi configurable via le panneau admin)

Des **personnages dotés d'une personnalité propre**, générés via l'IA. Chaque personnage a un avatar, une couleur d'embed, un rôle narratif et un style de réponse unique.

L'Owner peut configurer les personnages, leur accès et leurs paramètres via `/superadmin → 🧠 IA & Modèles`.

---

## 🎲 Pitch IA

### `/pitch`

> 🔒 **Accès :** Configurable

Découvrez un film ou une série selon vos envies grâce à l'IA.

**Modes disponibles :**
- **🎲 Aléatoire** — Surprise totale
- **🎭 Par genre** — Action · Aventure · Comédie · Horreur · Science-Fiction · ...
- **📅 Par époque** — Années 80 · 90 · 2000 · Récent...
- **🌡️ Par ambiance** — Feel-good · Intense · Dépaysant...
- **✏️ Manuel** — Décrivez ce que vous voulez, l'IA trouve

Le bot génère une **fiche complète** avec affiche, résumé, note et informations TMDB.

---

## 🔗 Liens utiles

### `/liensutiles`

> **Accès :** Tous les membres

Affiche un panneau de **liens utiles personnalisé** selon votre niveau d'accès.

Contenu standard :
- 📅 Lien vers le **calendrier en ligne** (GitHub Pages) (Autorisé pour tout le monde)
- 🎬 Lien vers la **liste des films 3D** disponibles (Disponible niveau N0 à N3)

> La réponse est **éphémère** (visible uniquement par vous).

---

## 🛡️ Administration

### `/admin`

> 🔒 **Accès :** AdminBot · Owner

Vue complète de **toutes les séances** (passées et à venir) avec annulation rapide.

- Chaque séance affiche : ID · Titre · Dates FR/QC · Salle · Diffuseur · Format
- Boutons 🗑️ rouges pour **annuler immédiatement** n'importe quelle séance
- Réponse éphémère · Timeout 3 minutes

---

### 🗑️ Reset des séances

> 🔒 **Accès :** AdminBot · Owner  
> ⚠️ **Toutes ces actions sont IRRÉVERSIBLES.** Faites toujours une sauvegarde avant !

Depuis `/superadmin → Outils N0 → 🔄 Reset` :

| Commande | Effet |
|----------|-------|
| **Reset total** | Supprime TOUTES les séances |
| **Reset par mois** | Ex. : toutes les séances de mars 2026 |
| **Reset par jour** | Ex. : toutes les séances du 15 mars 2026 |

---

### 💾 Sauvegarde

> 🔒 **Accès :** AdminBot · Owner

Depuis `/superadmin → Outils N0 → 💾 DB / Sauvegarde` :

| Format | Description |
|--------|-------------|
| **DB + CSV** ← recommandé | Les deux formats envoyés en DM |
| **SQLite (.db)** | Fichier brut restaurable via `/restaurer` |
| **CSV** | Tableau lisible dans Excel / Google Sheets |

> La sauvegarde est envoyée en **DM privé**, invisible aux autres membres.

---

### ♻️ Restauration

> 🔒 **Accès :** AdminBot · Owner  
> ⚠️ **ACTION IRRÉVERSIBLE** — Écrase intégralement toutes les données actuelles.

**Procédure recommandée :**

```
1. Sauvegarde → /superadmin → Outils N0 → 💾 DB / Sauvegarde
2. Restauration → /superadmin → Outils N0 → ♻️ Restaurer
3. Uploadez le fichier .db
4. Confirmez l'opération
```

Une **sauvegarde automatique** dans `seances.db.bak` est effectuée juste avant l'écrasement.

---

## 👑 SuperAdmin

### `/superadmin`

> 🔒 **Accès exclusif :** Owner uniquement

Panneau de contrôle complet avec **navigation par boutons** sur **8 pages**. Réponse éphémère.

---

### Page 1 — 🎛️ Fonctions

Gestion des niveaux d'accès par fonctionnalité :
- Activer / désactiver chaque fonction individuellement
- Configurer le niveau minimum requis (Owner · AdminBot · Admin · Diffuseur · Tous)
- Mode éphémère par fonction

---

### Page 2 — 🎮 /fun

Activer ou désactiver chaque sous-commande de `/fun` individuellement :
- Synopsis foireux · Note mystère · Qui est-ce ? · Trouve ton film
- Quiz IA · Citations · Aventure Kaamelott · Cross-Univers · Burger Quiz · Kamoulox
- Réactions · GIFs · Sondages

---

### Page 3 — 🖼️ GIF Config

Configuration des GIFs :
- Activer / désactiver Klipy et Giphy indépendamment
- Configurer les clés API directement depuis l'interface
- Toggles individuels : Statik · Popcorn · Kaamelott GIF

---

### Page 4 — 🧠 IA & Modèles

Configuration du moteur IA :
- Toggle global IA (active/désactive toutes les fonctions IA)
- Choix du moteur : **Groq** ou **Gemini**
- Visualisation du modèle actif et de l'état de chaque API
- Chaîne de fallback : Groq → Gemini → Réponses locales

---

### Page 5 — 📢 Rappels & DM

Configuration centralisée des communications automatiques :
- Toggle rappels ON/OFF
- Modification des paliers de rappel (en minutes)
- Toggle DM de confirmation pour le diffuseur
- Toggle DM de rappel pour le diffuseur

---

### Page 6 — 📺 Channels & Rôles

Configuration **sans toucher au code** :

**Salons :**
- Collez l'ID numérique ou mentionnez `#nom-du-salon`
- `#séances-à-venir` · `#calendrier`

**Rôles :**
- Nom du rôle AdminBot
- Liste des rôles autorisés à programmer

---

### Page 7 — 🎬 Quiz Citations

Paramètres du jeu Quiz Citations :
- Nombre de rounds (boutons ⬅️ ➡️)
- Temps de réponse par round
- Pause entre les rounds
- Niveau minimum requis

---

### Page 8 — 👁️ Vue par Rôle

**Simulation en temps réel** de ce que voit chaque membre selon son rôle :
- Commandes disponibles
- Fonctions `/fun` accessibles
- Aperçu de l'aide telle qu'elle s'affiche

---

### 🔁 Refresh des commandes

Depuis `/superadmin → Outils N0 → 🔁 Refresh` :

| Option | Description |
|--------|-------------|
| **🏠 Ce serveur** | Synchronisation instantanée sur votre serveur ✅ |
| **🌐 Global (~1h)** | Propagation globale Discord (lente) |
| **⚠️ Effacer & Resync** | Dernier recours — doublons ou commandes fantômes |
| **👥 Refresh membres** | Vide le cache des rôles et rescanne tous les membres |

---

### `/superadmin_restaurer`

> 🔒 **Accès exclusif :** Owner uniquement

Version renforcée de la restauration de base de données :
- Validation du fichier avant exécution
- Sauvegarde automatique dans `seances.db.bak`
- Site GitHub Pages régénéré automatiquement après restauration

---

## 📊 Statistiques

> **Accès :** Visible par tous les membres du salon

Disponibles depuis `/superadmin → Outils N0 → 📊 Statistiques` (version détaillée) :

| Statistique | Description |
|-------------|-------------|
| 🎬 Total séances | Nombre total de diffusions enregistrées |
| 📅 À venir | Séances à venir |
| 📼 Passées | Séances archivées |
| 🎞️ Titres uniques | Nombre de titres différents diffusés |
| ⭐ Note moyenne | Note TMDB moyenne de toutes les diffusions |
| 🕐 Heure populaire | Heure de début la plus fréquente |
| 📆 Mois le plus actif | Mois avec le plus de diffusions |
| 🥽 Format 3D / 📽️ 2D | Répartition des formats |
| 📂 Types | Répartition Films · Séries · Animés · Docs... |
| 📡 Top 5 Diffuseurs | Les diffuseurs les plus actifs |
| ⏱️ Temps total | Durée cumulée de toutes les diffusions |

---

## 🔧 Configuration technique

### Structure des fichiers

```
📁 bot/
├── bot.py                   # Point d'entrée principal
├── config.py                # ⚙️ Configuration (tokens, IDs, rôles)
├── database.py              # Gestion SQLite des séances
├── db_extra.py              # Requêtes avancées base de données
├── db_registry.py           # Registre des configurations
├── db_characters.py         # Base des personnages IA
├── db_cinema.py             # Cache des données cinéma
├── generateur_site.py       # Génération HTML du calendrier en ligne
├── github_pages.py          # Publication sur GitHub Pages
├── tmdb.py                  # API TMDB (films & séries)
├── search_engine.py         # Moteur de recherche multi-sources
├── logger.py                # Système de logs
│
└── 📁 cogs/                 # Modules fonctionnels
    ├── seances.py           # /programmer — Programmation des séances
    ├── admin_owner.py       # /gestion · /superadmin
    ├── superadmin.py        # Panneau SuperAdmin complet
    ├── aide.py              # /aide — Aide interactive
    ├── fun.py               # /fun — Tous les jeux et défis
    ├── fun_equipe_ia.py     # Personnages IA (Équipe)
    ├── quiz_ia.py           # Quiz IA · Kamoulox
    ├── quiz_citations.py    # /quiz_citations
    ├── kaamelott_aventure.py# Aventure Kaamelott
    ├── cinema_ia.py         # IA cinéma (Cross-Univers, Synopsis...)
    ├── pitch.py             # /pitch — Découverte de films
    ├── calendrier.py        # Calendrier Discord + GitHub Pages
    ├── rappels.py           # Rappels automatiques (tâche de fond)
    ├── liensutiles.py       # /liensutiles
    ├── sauvegarde.py        # Sauvegarde base de données
    ├── restauration.py      # Restauration base de données
    ├── reset.py             # Reset séances
    ├── social.py            # Fonctions sociales
    └── groupes.py           # Gestion des groupes
```

### Système de logs

Le bot génère des logs structurés dans `logs/` :

```
📁 logs/
├── bot_activity.log         # Toutes les activités
├── erreurs.log              # Erreurs uniquement
├── 📁 special/
│   ├── statik_permanent.txt # Historique Statik
│   └── kaamelott_permanent.txt # Historique GIFs Kaamelott
└── 📁 utilisateurs/
    └── user_[ID]_[pseudo].txt  # Log par utilisateur
```

### APIs utilisées

| Service | Usage | Lien |
|---------|-------|------|
| **Discord API** | Bot principal | [discord.com/developers](https://discord.com/developers) |
| **TMDB** | Métadonnées films & séries | [themoviedb.org](https://www.themoviedb.org/settings/api) |
| **OMDb** | Titres alternatifs | [omdbapi.com](https://www.omdbapi.com/) |
| **AniList** | Animés japonais/asiatiques | Automatique |
| **GitHub API** | Publication calendrier | [github.com/settings/tokens](https://github.com/settings/tokens) |
| **Klipy** | GIFs (principal) | [klipy.com/developers](https://klipy.com/developers) |
| **Giphy** | GIFs (secours) | [developers.giphy.com](https://developers.giphy.com/) |
| **Groq / Gemini** | Moteur IA | Configuration via `/superadmin` |

---

<div align="center">

**🦑🐙 Dreadlocktopus_Bot V5 🐙🦑**

*Fait avec passion pour la communauté cinéma BigscreenFR.fr*

</div>
