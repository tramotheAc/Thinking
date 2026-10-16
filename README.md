# Doctrine IA · données · applications

Dossier de synthèse de la DSI d'un bailleur social sur trois sujets liés :
**l'IA**, **l'entrepôt de données** et **le vibecoding** (les applications
produites avec l'aide de l'IA). Il rassemble une série d'échanges et la doctrine
qui en ressort.

Deux idées traversent tout le dossier :

- **différer ce qui est bon marché à changer, verrouiller ce qui est cher ou
  dangereux à rattraper** ;
- quand produire devient gratuit, la valeur se déplace **de l'objet vers le
  méta** : du « faire » vers « gouverner le sens et en répondre ».

C'est un **site statique d'une seule page** (`index.html`) : pas de framework,
pas de build, pas de dépendance. Les styles, les illustrations (SVG), le
bandeau et la navigation sont dans le fichier. Seules les polices viennent de
Google Fonts ; hors ligne, la page reste lisible avec les polices de repli.

---

## Les cinq thématiques

Une page d'accueil, puis cinq pages. Chacune s'ouvre sur une analogie de la vie
courante, la rapproche de la situation de la DSI et se termine par un encadré
« Ce qu'on retient ».

| # | Page | Analogie | Thèse |
|---|---|---|---|
| 1 | **IA & MCP** | La lampe torche | L'interface conversationnelle n'est pas une fin. Le MCP expose des capacités métier adossées à une couche sémantique gouvernée, et pas du « SQL en langage naturel ». |
| 2 | **Applications & conteneurs** | Le lotissement | Industrialiser sans brider le vibecoding : la surface est libre, le contrat (identité, sécurité, données, exploitation) est verrouillé. Le paved road, avec des glissières plutôt que des portails. |
| 3 | **Typologie & shadow** | Les travaux dans un logement | On classe les applis selon ce qu'elles touchent (0 volant / 1 lecture / 2 écriture). La DSI doit savoir dire oui vite pour faire sortir le shadow vibecoding de l'ombre. |
| 4 | **Métier & compétences** | De l'argentique au numérique | Le chef de projet data dans 5 à 10 ans : ce qui prend de la valeur (sémantique, vérification, orchestration d'agents) et ce qui en perd. |
| 5 | **Atrophie & effets de bord** | Le GPS / Waze | Perte de la carte mentale, second bouchon, boucle de rétroaction. L'humain reste la voix qui peut refuser de suivre. |

Chaque page a sa propre adresse (`#ia`, `#apps`, `#typo`, `#metier`, `#waze`),
qu'on peut partager ou mettre en favori. L'impression (`Ctrl+P`) produit tout le
dossier d'un bloc, une page par thématique.

---

## Ouvrir en local

**Sans rien installer** : double-cliquer sur `index.html`. La page fonctionne
directement depuis le disque (`file://`).

**Avec un petit serveur** (Node.js ≥ 18) : utile pour voir la page comme elle
sera servie en ligne.

```bash
npm start
```

La page est alors sur <http://localhost:3000>. La commande lance
[`serve`](https://github.com/vercel/serve) via `npx`, dans une version figée.
Rien ne s'installe dans le projet et il n'y a pas de `node_modules`. Au premier
lancement, `npx` télécharge `serve` dans son cache.

Sans Node, n'importe quel serveur statique fait l'affaire, par exemple
`python3 -m http.server 3000`.

---

## Déployer

Le site est le seul fichier `index.html`. Tout hébergement de fichiers statiques
convient.

- **GitHub Pages** : *Settings → Pages → Build and deployment*, source
  « Deploy from a branch », branche `main`, dossier `/ (root)`. Le site est
  publié à `https://<compte>.github.io/<dépôt>/`.
- **Azure Static Web Apps / Azure Storage (site statique)** : déposer
  `index.html` à la racine du conteneur `$web` ou de l'application. Il n'y a
  aucune commande de build et le dossier de sortie est la racine.
- **Serveur web interne** (IIS, nginx, Apache) : copier `index.html` dans le
  dossier publié. Aucune réécriture d'URL n'est nécessaire, car la navigation
  passe par l'ancre (`#…`) et ne fait jamais d'appel au serveur.

> Le dossier est un **document de travail**. Avant de le publier hors du réseau
> interne, vérifier que son contenu peut sortir.

---

## Structure

```
index.html     le dossier complet : styles (charte), illustrations SVG, navigation
package.json   le seul script « start » (serveur local)
README.md      ce fichier
.gitignore
```

## Mise en forme

La page applique la **charte visuelle Aiguillon** (dépôt `charte-visuelle`).
Ses jetons de couleur (`jetons.css`) sont repris à l'identique dans le
`<style>`.

**Navigation.** Une barre collée en haut, aux bleus du bandeau de la charte :

- six onglets, avec un curseur blanc qui glisse sous la page active ;
- un filet sous la barre qui suit l'avancée de la lecture ;
- sous 1080 px, les onglets laissent place à un bouton qui indique la page en
  cours et déplie un **volet** avec les six pages en cartes ;
- en bas de chaque page, des liens vers la page précédente et la suivante ;
- au clavier : `←` / `→` pour changer de page, `Échap` pour refermer le volet ;
- la page glisse du côté où l'on va, et ses blocs apparaissent en cascade
  (mouvements repris de l'application Oukilest, dépôt `SIG`).

**Illustrations.** Six scènes en SVG, dessinées pour le dossier (lampe torche,
lotissement, travaux, argentique, GPS, ligne de partage), plus les icônes des
cartes. Elles sont animées discrètement : flux dans le réseau commun, signaux
faibles qui pulsent, voitures qui piétinent, boucle qui tourne. Leur fond et
leurs textes suivent le thème. Sur téléphone, elles gardent une largeur lisible
et se font défiler du doigt dans leur carte.

**Thème sombre.** Il suit le réglage du poste, et le bouton soleil / lune de la
barre (`bouton-theme` de la charte) permet de le forcer. Le choix n'est pas
mémorisé. Toutes les animations sont coupées si le poste demande moins de
mouvement.

Après une évolution de la charte, on vérifie et on resynchronise depuis le clone
de la charte :

```bash
python3 <charte>/outils/couverture.py index.html   # aucun jeton manquant ?
python3 <charte>/outils/theme.py index.html        # reprend les nouveaux jetons
```

## Modifier le contenu

Chaque page est une `<section class="page" id="…">` dans `index.html`. Les
onglets pointent vers ces identifiants par `data-go="…"`. Pour ajouter une page,
il faut une section, un onglet qui porte le même identifiant et une carte sur
l'accueil, que le volet reprend toute seule. Le script recalcule les liens
« précédente / suivante » d'après l'ordre des sections.
