# MyBoard — Tableau blanc collaboratif

Application de tableau blanc avec compte utilisateur et sauvegarde cloud via Supabase.

## Structure du projet

```
myboard/
├── index.html      → App principale (tableau blanc)
├── login.html      → Page de connexion / inscription
├── config.js       → Clés Supabase
└── vercel.json     → Config routage Vercel
```

## Déploiement sur Vercel

### Méthode 1 — Via l'interface Vercel (recommandé)

1. Va sur **vercel.com** → crée un compte gratuit
2. "Add New Project" → "Import Git Repository"
3. Ou plus simple : clique "Deploy" et glisse-dépose le dossier `myboard`

### Méthode 2 — Via CLI

```bash
npm i -g vercel
cd myboard
vercel
```

## Supabase — Table requise

Colle ça dans SQL Editor → Run :

```sql
create table boards (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users not null,
  name text not null default 'Nouveau tableau',
  data jsonb not null default '{}',
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

alter table boards enable row level security;

create policy "Users see own boards"
on boards for all using (auth.uid() = user_id)
with check (auth.uid() = user_id);
```

## Activer Google OAuth (optionnel)

Dans Supabase → Authentication → Providers → Google :
- Active Google
- Ajoute l'URL de redirect : `https://TON-DOMAINE.vercel.app/`
- Configure les credentials Google OAuth Console

## Fonctionnalités

- ✅ Compte utilisateur (email + mot de passe + Google)
- ✅ Sauvegarde automatique dans le cloud (1.5s après modification)
- ✅ Multi-tableaux avec onglets
- ✅ Cartes colorées pleines style Miro
- ✅ Post-its, texte libre, flèches, dessin libre
- ✅ Import d'images (glisser-déposer ou bouton)
- ✅ Redimensionnement proportionnel des images
- ✅ Export PNG haute résolution (×3)
- ✅ Export/Import JSON
- ✅ Mode présentation
- ✅ HiDPI / Retina (texte et éléments ultra nets)
- ✅ Raccourcis clavier complets
