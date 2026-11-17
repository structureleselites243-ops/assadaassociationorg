# ASSADA — Site web + Supabase

Site responsive en français pour ASSADA, avec présentation des missions, coordonnées, formulaire de contact, actualités publiques et espace d'administration privé.

## 1. Démarrer le site

Prérequis : Node.js 20 ou plus récent.

```bash
npm install
cp .env.example .env
npm run dev
```

Ouvrez l'adresse locale affichée par Vite.

## 2. Créer et connecter Supabase

1. Créez un projet sur https://supabase.com.
2. Dans **Project Settings → API**, copiez l'URL du projet et la clé **publishable** (ou l'ancienne clé `anon`). Ne mettez JAMAIS la clé `service_role` dans le navigateur.
3. Dans le dossier du projet, créez `.env` à partir de `.env.example` et renseignez :

```env
VITE_SUPABASE_URL=https://VOTRE-PROJET.supabase.co
VITE_SUPABASE_ANON_KEY=VOTRE_CLE_PUBLISHABLE_OU_ANON
```

4. Dans Supabase → **SQL Editor**, ouvrez `supabase/schema.sql` et exécutez le script.
5. Dans Supabase → **Authentication → Users**, créez votre utilisateur administrateur avec votre propre adresse e-mail et un mot de passe fort. N'activez pas l'inscription publique.
6. Copiez l'UUID de cet utilisateur. Dans SQL Editor, exécutez en remplaçant l'UUID :

```sql
insert into public.admin_users (user_id)
values ('COLLER-ICI-UUID-DE-VOTRE-UTILISATEUR');
```

7. Relancez le site (`npm run dev`). Le bouton **ADMIN** ouvre `/admin`.

La table `admin_users` fait autorité : seul un utilisateur dont l'UUID y figure est administrateur. Les règles RLS protègent les données, même si quelqu'un tente d'appeler directement l'API. Le formulaire public peut uniquement ajouter des messages ; leur lecture est réservée à l'administrateur.

## 3. Déployer

Le projet peut être déployé sur Vercel ou Netlify :
- Build command : `npm run build`
- Output directory : `dist`
- Variables d'environnement : `VITE_SUPABASE_URL` et `VITE_SUPABASE_ANON_KEY`

Ajoutez également le domaine de production dans Supabase → Authentication → URL Configuration si vous utilisez les liens de récupération de mot de passe plus tard.

## Fonctionnalités incluses

- Site public responsive et identité visuelle inspirée de la bannière fournie.
- Présentation d'ASSADA et de ses cinq missions.
- Coordonnées et formulaire de contact connecté à Supabase.
- Affichage des actualités publiées.
- Connexion administrateur Supabase Auth (sans inscription publique).
- Tableau de bord : publier, modifier et supprimer les actualités ; consulter les messages reçus.

## Sécurité / limites de cette première version

- Ne partagez jamais la clé `service_role`.
- Créez le compte administrateur depuis le tableau de bord Supabase, pas depuis le site.
- Configurez un CAPTCHA ou une protection anti-spam avant une mise en production à fort trafic.
- Les coordonnées affichées reprennent celles de la bannière fournie ; vérifiez qu'elles sont toujours exactes avant publication.
