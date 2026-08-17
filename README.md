# Msomo — Système de Gestion Scolaire

Application de gestion scolaire pour les écoles des Comores. Gestion des classes, élèves, présences, évaluations, notes et bulletins.

## Stack technique

| Composant | Technologie |
|---|---|
| **Frontend** | React 18 + TypeScript + Vite + Chakra UI |
| **Backend** | NestJS + TypeScript + Sequelize ORM |
| **PDF Service** | Python 3.11 + FastAPI + WeasyPrint |
| **Base de données** | PostgreSQL 16 |
| **Desktop** | Tauri v2 (installeurs NSIS/MSI) |
| **Offline** | Dexie.js (IndexedDB) + sync queue |

## Structure du projet

```
msomo/
├── frontend/          # React SPA (port 5173)
├── backend/           # API NestJS (port 4000)
├── pdf-service/       # Microservice PDF (port 8000)
├── docker-compose.yml # PostgreSQL + pdf-service
└── README.md
```

## Prérequis

- Node.js >= 18
- Docker & Docker Compose
- Python 3.11+
- Git

## Installation

### 1. Démarrer la base de données

```bash
docker-compose up -d postgres
```

Le schéma SQL est appliqué automatiquement au premier démarrage.

### 2. Backend

```bash
cd backend
npm install
cp .env.example .env   # ou configurer .env manuellement
npm run dev
```

Le serveur démarre sur `http://localhost:4000`. L'admin par défaut est créé automatiquement.

### 3. Frontend

```bash
cd frontend
npm install
npm run dev
```

L'app démarre sur `http://localhost:5173`.

### 4. PDF Service

```bash
cd pdf-service
pip install -r requirements.txt
python main.py
```

Ou via Docker :

```bash
docker-compose up -d pdf-service
```

Le service est accessible sur `http://localhost:8000`.

## Rôles utilisateurs

| Rôle | Périmètre |
|---|---|
| **Directeur** | Gestion complète : établissement, classes, élèves, enseignants, résultats |
| **Enseignant** | Sa classe : emploi du temps, présences, notes, bulletins |

## Versions

| Version | Périmètre |
|---|---|
| **V1 (MVP)** | Une classe, un trimestre, deux rôles — présences, notes, bulletins PDF |
| **V2** | Structure pédagogique complète (cycles, niveaux, multi-classes) |
| **V3** | Admissions et inscriptions |
| **V4** | Finances (frais, paiements, reçus) |
| **V5** | Fin d'année (conseil de classe, décisions de passage) |
| **V6** | Portails Élève et Responsable |
| **V7** | Administration et conformité (audit log) |
| **V8** | Offline et multi-plateforme (Tauri, mobile) |
| **V9** | Remplacements et gestion avancée |

## Licence

Projet privé — Lawi Ibrahim Salim
