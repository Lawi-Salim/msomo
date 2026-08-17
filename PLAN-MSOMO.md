# PLAN-MSOMO — Scaffolding Phase 0 + Phase 1 (MVP)

## Objectif

Créer la structure complète du projet Msomo (Système de Gestion Scolaire) en réutilisant l'architecture d'Avipoul, en y ajoutant le README.md, et en couvrant Phase 0 (socle technique) + Phase 1 (MVP).

## Décisions validées

- **Périmètre** : Phase 0 + Phase 1 MVP + README.md
- **Offline** : Inclure le système Dexie/IndexedDB (copié/adapté d'Avipoul)
- **Desktop** : Inclure la config Tauri v2
- **Thème** : Garder le ColorTheme.tsx Msomo Blue existant, adapter le reste autour

---

## Fichiers à créer (par ordre)

### 1. Racine du projet

| Fichier | Source | Notes |
|---|---|---|
| `README.md` | Nouveau | Description du projet Msomo |
| `.gitignore` | Copie Avipoul | Adapté (pas de backup-daily.bat au même endroit) |
| `.vscode/settings.json` | Copie Avipoul | Changer `avipoul` → `msomo` dans la DB |
| `.vscode/launch.json` | Copie Avipoul | Adapter les paths |
| `docker-compose.yml` | Nouveau | PostgreSQL + pdf-service |
| `backup-daily.bat` | Copie Avipoul | Adapté pour Msomo |

### 2. Backend (`backend/`)

| Fichier | Source | Adaptations |
|---|---|---|
| `package.json` | Copie Avipoul | Nom → `msomo-backend` |
| `tsconfig.json` | Copie Avipoul | Identique |
| `.env` | Copie Avipoul | `DATABASE_URL` → `msomo`, `JWT_SECRET` nouveau |
| `src/main.ts` | Copie Avipoul | Port 4000, CORS adapter |
| `src/app.module.ts` | Adapté | Modules Msomo au lieu d'Avipoul |
| `src/types/bcryptjs.d.ts` | Copie Avipoul | Identique |

#### Modules réutilisables (copie quasi identique d'Avipoul)

| Module | Fichiers | Adaptations |
|---|---|---|
| `auth/` | `auth.module.ts`, `auth.controller.ts`, `auth.service.ts`, `user.entity.ts`, `jwt.strategy.ts`, `jwt-auth.guard.ts`, `roles.guard.ts`, `roles.decorator.ts`, `dto/login.dto.ts`, `dto/register.dto.ts`, `utilisateurs.controller.ts` | Rôles: `directeur`, `enseignant` (au lieu de `admin`, `employe`, `comptable`) |
| `common/constants.ts` | Copie Avipoul | Adapter paths si nécessaire |
| `common/jobs/` | `jobs.module.ts`, `jobs.controller.ts`, `verifications.job.ts` | Simplifié pour MVP (pas de vérifications stock) |
| `seed/admin.seed.ts` | Copie Avipoul | Adapter le rôle par défaut |

#### Nouveaux modules métier (Phase 1 MVP)

| Module | Fichiers | Tables SQL |
|---|---|---|
| `etablissements/` | `etablissements.module.ts`, `.controller.ts`, `.service.ts`, `etablissement.entity.ts`, `dto/create-etablissement.dto.ts` | `etablissements` |
| `annees-scolaires/` | `annees-scolaires.module.ts`, `.controller.ts`, `.service.ts`, `annee-scolaire.entity.ts`, `dto/create-annee-scolaire.dto.ts` | `annees_scolaires` |
| `classes/` | `classes.module.ts`, `.controller.ts`, `.service.ts`, `classe.entity.ts`, `dto/create-classe.dto.ts` | `classes` |
| `eleves/` | `eleves.module.ts`, `.controller.ts`, `.service.ts`, `eleve.entity.ts`, `dto/create-eleve.dto.ts` | `eleves` |
| `matieres/` | `matieres.module.ts`, `.controller.ts`, `.service.ts`, `matiere.entity.ts`, `dto/create-matiere.dto.ts` | `matieres` |
| `emplois-du-temps/` | `emplois-du-temps.module.ts`, `.controller.ts`, `.service.ts`, `emploi-du-temps.entity.ts`, `dto/create-emploi-du-temps.dto.ts` | `emplois_du_temps` |
| `presences/` | `presences.module.ts`, `.controller.ts`, `.service.ts`, `presence.entity.ts`, `dto/create-presence.dto.ts` | `presences` |
| `evaluations/` | `evaluations.module.ts`, `.controller.ts`, `.service.ts`, `evaluation.entity.ts`, `dto/create-evaluation.dto.ts` | `evaluations` |
| `notes/` | `notes.module.ts`, `.controller.ts`, `.service.ts`, `note.entity.ts`, `dto/create-note.dto.ts` | `notes` |
| `documents/` | `documents.module.ts`, `.controller.ts` | Appel au pdf-service |
| `rapports/` | `rapports.module.ts`, `.controller.ts` | Génération PDF |

### 3. Frontend (`frontend/`)

| Fichier | Source | Adaptations |
|---|---|---|
| `package.json` | Copie Avipoul | Nom → `msomo-frontend` |
| `tsconfig.json` | Copie Avipoul | Identique |
| `vite.config.ts` | Copie Avipoul | Identique |
| `index.html` | Copie Avipoul | Titre → `Msomo` |
| `.env.development` | Copie Avipoul | `VITE_API_URL=http://localhost:4000` |

#### `src/main.tsx`
Copie d'Avipoul avec la même hiérarchie de providers :
```
StrictMode > ColorModeScript > ChakraProvider > BrowserRouter > AuthProvider > ConnectivityProvider > DownloadProvider > App
```

#### `src/App.tsx`
Routes adaptées au MVP Msomo :
- `/login` → Login
- `/` → Dashboard (Directeur)
- `/etablissement` → Etablissement
- `/annees-scolaires` → AnneesScolaires
- `/classes` → Classes
- `/classes/:id/eleves` → Eleves (liste d'une classe)
- `/emplois-du-temps` → EmploiDuTemps
- `/presences` → Presences
- `/evaluations` → Evaluations
- `/notes/:evaluationId` → Notes (saisie des notes d'une évaluation)
- `/resultats` → Resultats (moyennes, classement)
- `/bulletin/:eleveId` → BulletinPreview
- `/utilisateurs` → Utilisateurs (admin only)
- `*` → NotFound

#### Composants réutilisables (copie d'Avipoul)

| Composant | Fichier | Adaptations |
|---|---|---|
| `DashboardLayout` | `components/layout/DashboardLayout.tsx` | Sidebar items adaptés à Msomo |
| `SidebarMobile` | `components/layout/SidebarMobile.tsx` | Identique |
| `Navbar` | `components/Navbar.tsx` | Identique |
| `ConfirmModal` | `components/ConfirmModal.tsx` | Identique |
| `AppToast` | `components/AppToast.tsx` | Identique |
| `Pagination` | `components/Pagination.tsx` | Identique |
| `PageLoading` | `components/PageLoading.tsx` | Identique |
| `TableLoading` | `components/TableLoading.tsx` | Identique |
| `OfflineBanner` | `components/OfflineBanner.tsx` | Identique |
| `Layout` | `components/Layout.tsx` | Identique |
| `Search` | `components/search/Search.tsx` | Identique |

#### Contexts (copie d'Avipoul)

| Contexte | Fichier | Adaptations |
|---|---|---|
| `AuthProvider` | `contexts/AuthContext.tsx` | Identique |
| `ConnectivityProvider` | `contexts/ConnectivityContext.tsx` | Identique |
| `DownloadProvider` | `contexts/DownloadContext.tsx` | Identique |

#### Services (copie/adaptation d'Avipoul)

| Service | Fichier | Adaptations |
|---|---|---|
| `api.ts` | `services/api.ts` | Identique (Axios + interceptors + offline queue) |
| `auth.service.ts` | `services/auth.service.ts` | Identique |
| `etablissements.service.ts` | Nouveau | CRUD établissements |
| `annees-scolaires.service.ts` | Nouveau | CRUD années scolaires |
| `classes.service.ts` | Nouveau | CRUD classes |
| `eleves.service.ts` | Nouveau | CRUD élèves |
| `matieres.service.ts` | Nouveau | CRUD matières |
| `emplois-du-temps.service.ts` | Nouveau | CRUD emplois du temps |
| `presences.service.ts` | Nouveau | CRUD présences |
| `evaluations.service.ts` | Nouveau | CRUD évaluations |
| `notes.service.ts` | Nouveau | CRUD notes + calcul moyennes |
| `documents.service.ts` | Nouveau | Génération PDF bulletins |

#### Hooks

| Hook | Fichier | Source |
|---|---|---|
| `useOfflineData` | `hooks/useOfflineData.ts` | Copie Avipoul |

#### Theme (garder Msomo Blue)

| Fichier | Source |
|---|---|
| `theme/ColorTheme.tsx` | **EXISTANT** — garder tel quel |
| `theme/designTokens.ts` | Adapté depuis Avipoul (changer les couleurs d'accent pour Msomo Blue) |
| `theme/ThemeMode.tsx` | Copie Avipoul (identique) |

#### Utils (copie d'Avipoul)

| Util | Fichier |
|---|---|
| `uuid.ts` | Identique |
| `deviceId.ts` | Identique |
| `downloadFile.ts` | Identique |
| `creatorLabel.ts` | Identique |
| `apiAuth.ts` | Identique |

#### DB / Offline (copie d'Avipoul, adapté)

| Fichier | Adaptations |
|---|---|
| `db/database.ts` | Tables Dexie adaptées aux tables Msomo |
| `db/cache.ts` | Identique (logique générique) |
| `db/hydrate.ts` | Identique |
| `db/syncQueue.ts` | Identique |
| `db/conflits.ts` | Identique |
| `db/connectivity.ts` | Identique |

#### Pages (nouvelles, spécifiques Msomo MVP)

| Page | Description |
|---|---|
| `Login.tsx` | Page de connexion (adaptée depuis Avipoul) |
| `Dashboard.tsx` | Tableau de bord Directeur (adapté) |
| `Etablissement.tsx` | Créer/consulter l'établissement |
| `AnneesScolaires.tsx` | Gestion des années scolaires |
| `Classes.tsx` | Liste des classes |
| `Eleves.tsx` | Liste des élèves d'une classe |
| `EmploiDuTemps.tsx` | Emploi du temps (enseignant) |
| `Presences.tsx` | Marquer présences/absences |
| `Evaluations.tsx` | Créer/lister évaluations |
| `Notes.tsx` | Saisir notes d'une évaluation |
| `Resultats.tsx` | Moyennes et classement |
| `BulletinPreview.tsx` | Aperçu bulletin PDF |
| `Utilisateurs.tsx` | CRUD utilisateurs (admin) |
| `NotFound.tsx` | Page 404 (copie Avipoul) |

#### Assets

| Fichier | Source |
|---|---|
| `assets/fonts/` | Copier les polices Geist depuis Avipoul |
| `assets/img/logo.png` | Logo Msomo (à créer ou utiliser placeholder) |
| `assets/img/favicon-svg.svg` | Adapté |

### 4. PDF Service (`pdf-service/`)

| Fichier | Source | Adaptations |
|---|---|---|
| `main.py` | Adapté | Endpoint `/bulletin` au lieu de `/rapport-cycle` et `/facture` |
| `requirements.txt` | Copie Avipoul | Identique |
| `Dockerfile` | Copie Avipoul | Identique |
| `docker-compose.yml` | Copie Avipoul | Identique |
| `templates/bulletin.html` | Nouveau | Template Jinja2 pour bulletin scolaire |
| `static/` | Copie logo | Logo pour les bulletins |

### 5. README.md (nouveau)

Description complète du projet Msomo :
- Nom, description, stack technique
- Structure du projet
- Prérequis (Node, Docker, Python)
- Guide d'installation
- Guide de développement
- Rôles utilisateurs

---

## Ordre d'exécution

1. **Racine** : `.gitignore`, `.vscode/`, `docker-compose.yml`, `README.md`
2. **Backend** : `package.json`, `tsconfig.json`, `.env`, `main.ts`, `auth/` complet, `common/`, `seed/`, puis modules métier un par un
3. **Frontend** : `package.json`, `tsconfig.json`, `vite.config.ts`, `index.html`, `.env.development`, `main.tsx`, `App.tsx`, puis composants réutilisables, contexts, services, hooks, theme, utils, db, pages
4. **PDF Service** : `main.py`, `requirements.txt`, `Dockerfile`, `templates/bulletin.html`

## Total estimé : ~120 fichiers
