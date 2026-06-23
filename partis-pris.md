# Partis pris techniques & regard de partenaire

## Partis pris techniques

### Format démo HTML/JS pur

Choix délibéré de livrer une démo statique (HTML/CSS/JS, sans framework) plutôt qu'une application Next.js complète.

**Pourquoi :** L'objectif du test est de démontrer la capacité à transposer fidèlement un simulateur existant — fonctionnellement et visuellement — dans un délai court. Un build Next.js aurait ajouté une couche de complexité (routing, SSR, bundler) sans valeur ajoutée pour ce périmètre.

**Conséquence :** Le livrable est un dossier `dist/` — aucune dépendance à installer, aucun build step. `npx serve dist/` suffit.

**Pour une intégration production dans `simulateurs.sinvestir.fr`** (stack Next.js/Vercel) : la logique métier (calculs DCA, appels CoinGecko, rendu Chart.js) est isolée et facilement extractible en composant React. Le choix HTML pur n'est pas un obstacle à la migration.

### Stack

| Choix | Raison |
|---|---|
| Tailwind CSS via CDN | Zéro build step, tokens visuels S'investir appliqués directement |
| Chart.js via CDN | Librairie légère, graphiques ligne + barres natifs, pas de D3 surdimensionné |
| CoinGecko API publique | Données temps réel sans clé API, couvre BTC/ETH/SOL et +10 000 actifs |
| `widget.html` séparé | Embarquable en `<iframe>` depuis n'importe quel contexte (sinvestir.fr, email, partenaires) |
| Zéro backend | Simulateur entièrement client-side — déploiement Vercel statique, coût nul |

---

## Regard de partenaire

### 1. Liens d'affiliation par simulateur

Chaque simulateur est associé à un besoin financier précis — c'est une opportunité de monétisation directe, sans friction pour l'utilisateur.

Exemples concrets :

| Simulateur | Plateformes cibles |
|---|---|
| Crypto | Binance, Coinbase, Trade Republic |
| Bourse / ETF | Degiro, Trade Republic, Fortuneo |
| Immobilier | Pretto, Meilleurtaux, SeLoger |
| Assurance-vie / épargne | Linxea, Yomoni, Boursorama |

Implémentation : 1 bloc CTA par simulateur, lien affilié paramétré (`?ref=sinvestir`). Coût technique : négligeable. Potentiel revenu récurrent : fort sur un trafic qualifié (utilisateurs en démarche active d'investissement).

### 3. Capture lead intégrée au simulateur

L'utilisateur qui vient de simuler un DCA BTC sur 3 ans a un contexte personnel fort — c'est le meilleur moment pour collecter son email.

Proposition : bouton "Recevoir mon analyse par email" déclenche un formulaire léger (prénom + email) → push HubSpot (déjà dans la stack S'investir) → séquence email automatisée avec récapitulatif de la simulation + ressources pédagogiques.

Avantages :
- Simulateur devient outil d'acquisition, pas seulement de contenu
- Lead ultra-qualifié (intention déclarée, chiffres personnels déjà saisis)
- Intégration HubSpot native, zéro nouveau outil

### 5. Comparaison multi-actifs

Permettre de comparer BTC vs ETH vs S&P 500 (ou autre indice) sur la même période et le même investissement mensuel.

Valeur pédagogique forte pour S'investir (positionnement éducation financière) : l'utilisateur visualise immédiatement la différence de performance et de volatilité entre classes d'actifs. Différenciateur clair vs simulateurs crypto concurrents qui restent mono-actif.

Implémentation dans l'architecture actuelle : ajout d'un second dropdown + second dataset sur le graphique Chart.js existant. Estimation : 1-2 jours de développement.
