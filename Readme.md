# Cours complet JavaScript : Boucles, DOM et Événements

---

## Partie 1 — Les boucles

Les boucles permettent de répéter un bloc de code plusieurs fois, tant qu'une condition est vraie ou pour chaque élément d'une collection.

### 1.1 La boucle `for`

C'est la boucle classique, utilisée quand on connaît (ou peut calculer) le nombre d'itérations.

```javascript
for (let i = 0; i < 5; i++) {
  console.log("Itération numéro : " + i);
}
// Affiche 0, 1, 2, 3, 4
```

Structure : `for (initialisation; condition; incrémentation) { ... }`
- **Initialisation** : exécutée une seule fois au début (`let i = 0`)
- **Condition** : vérifiée avant chaque tour, la boucle continue tant qu'elle est vraie
- **Incrémentation** : exécutée à la fin de chaque tour

**Exemple : parcourir un tableau**
```javascript
const fruits = ["pomme", "banane", "mangue"];
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}
```

### 1.2 La boucle `while`

Utilisée quand on ne connaît pas à l'avance le nombre de tours, mais une condition d'arrêt.

```javascript
let compteur = 0;
while (compteur < 3) {
  console.log("Compteur : " + compteur);
  compteur++;
}
```

⚠️ Attention à toujours faire évoluer la condition, sinon vous créez une **boucle infinie**.

### 1.3 La boucle `do...while`

Comme `while`, mais le bloc s'exécute **au moins une fois**, car la condition est vérifiée après.

```javascript
let x = 10;
do {
  console.log("x vaut : " + x);
  x++;
} while (x < 5);
// Affiche "x vaut : 10" une seule fois, même si la condition est fausse dès le départ
```

### 1.4 La boucle `for...of`

Idéale pour parcourir les **valeurs** d'un objet itérable (tableau, chaîne de caractères, Map, Set...).

```javascript
const couleurs = ["rouge", "vert", "bleu"];
for (const couleur of couleurs) {
  console.log(couleur);
}

// Fonctionne aussi sur les chaînes
for (const lettre of "salut") {
  console.log(lettre);
}
```

### 1.5 La boucle `for...in`

Parcourt les **clés** (propriétés énumérables) d'un objet. À utiliser principalement pour les objets, pas les tableaux.

```javascript
const personne = { nom: "Awa", age: 25, ville: "Yaoundé" };
for (const cle in personne) {
  console.log(cle + " : " + personne[cle]);
}
// nom : Awa
// age : 25
// ville : Yaoundé
```

### 1.6 `forEach` (méthode de tableau)

Ce n'est pas une boucle au sens strict, mais une méthode qui exécute une fonction pour chaque élément.

```javascript
const nombres = [1, 2, 3, 4];
nombres.forEach(function (nombre, index) {
  console.log(`Élément ${index} : ${nombre}`);
});

// Avec une fonction fléchée
nombres.forEach((n) => console.log(n * 2));
```

Différence clé : on ne peut pas utiliser `break` ou `continue` dans un `forEach` (contrairement à `for`).

### 1.7 `break` et `continue`

```javascript
// break : arrête complètement la boucle
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i); // 0,1,2,3,4
}

// continue : passe au tour suivant sans exécuter le reste du bloc
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;
  console.log(i); // 0,1,3,4
}
```

### 1.8 Tableau récapitulatif

| Boucle | Utilisation typique |
|---|---|
| `for` | Nombre d'itérations connu |
| `while` | Condition d'arrêt sans nombre de tours fixe |
| `do...while` | Comme `while`, mais exécution garantie au moins une fois |
| `for...of` | Parcourir les valeurs d'un itérable |
| `for...in` | Parcourir les clés d'un objet |
| `forEach` | Traiter chaque élément d'un tableau (sans break/continue) |

---

## Partie 2 — Le DOM (Document Object Model)

### 2.1 Qu'est-ce que le DOM ?

Le **DOM** est une représentation en arbre de la page HTML, que JavaScript peut lire et modifier. Chaque balise HTML devient un **nœud** (node) que l'on peut sélectionner, modifier, supprimer ou créer.

```
document
 └── html
      ├── head
      └── body
           ├── h1
           ├── p
           └── div
```

### 2.2 Sélectionner des éléments

```javascript
// Par id
const titre = document.getElementById("titre");

// Par classe (retourne une collection)
const items = document.getElementsByClassName("item");

// Par balise
const paragraphes = document.getElementsByTagName("p");

// querySelector : le premier élément qui correspond au sélecteur CSS
const bouton = document.querySelector(".btn-principal");
const premierLi = document.querySelector("ul li");

// querySelectorAll : tous les éléments correspondants (NodeList)
const tousLesBoutons = document.querySelectorAll("button");
```

`querySelector` / `querySelectorAll` sont les plus utilisés aujourd'hui car ils acceptent n'importe quel sélecteur CSS.

### 2.3 Modifier le contenu

```javascript
const div = document.querySelector("#maDiv");

// Modifier le texte
div.textContent = "Nouveau texte";

// Modifier le HTML interne (attention aux failles XSS avec du contenu utilisateur)
div.innerHTML = "<strong>Texte en gras</strong>";

// Modifier un attribut
div.setAttribute("data-status", "actif");
const status = div.getAttribute("data-status");

// Modifier le style directement
div.style.color = "blue";
div.style.backgroundColor = "#f0f0f0";
```

### 2.4 Manipuler les classes CSS

```javascript
const el = document.querySelector(".carte");

el.classList.add("active");        // ajoute une classe
el.classList.remove("hidden");     // supprime une classe
el.classList.toggle("selected");   // ajoute si absente, supprime si présente
el.classList.contains("active");   // renvoie true/false
```

### 2.5 Créer, insérer et supprimer des éléments

```javascript
// Créer un élément
const nouveauLi = document.createElement("li");
nouveauLi.textContent = "Nouvel élément";

// L'ajouter dans le DOM
const liste = document.querySelector("ul");
liste.appendChild(nouveauLi);          // ajoute à la fin
liste.prepend(nouveauLi);              // ajoute au début
liste.insertBefore(nouveauLi, liste.children[2]); // avant un élément précis

// Supprimer un élément
nouveauLi.remove();

// Ancienne méthode (encore utile à connaître)
liste.removeChild(nouveauLi);
```

### 2.6 Parcourir les parents / enfants

```javascript
const el = document.querySelector(".item");

el.parentElement;      // élément parent
el.children;           // enfants directs (HTMLCollection)
el.firstElementChild;  // premier enfant
el.lastElementChild;   // dernier enfant
el.nextElementSibling; // élément suivant au même niveau
el.previousElementSibling; // élément précédent
```

---

## Partie 3 — Les événements JavaScript

### 3.1 Qu'est-ce qu'un événement ?

Un événement est une action détectée par le navigateur : clic, appui sur une touche, chargement de page, soumission de formulaire, etc. JavaScript permet d'**écouter** ces événements et de réagir avec une fonction.

### 3.2 `addEventListener`

C'est la méthode moderne et recommandée pour gérer les événements.

```javascript
const bouton = document.querySelector("#monBouton");

bouton.addEventListener("click", function () {
  console.log("Bouton cliqué !");
});

// Avec une fonction fléchée
bouton.addEventListener("click", () => {
  alert("Merci d'avoir cliqué !");
});
```

Pourquoi préférer `addEventListener` à `onclick="..."` en HTML ou `element.onclick = ...` ?
- On peut attacher **plusieurs écouteurs** au même événement
- Séparation claire entre HTML et JavaScript
- Plus d'options de contrôle (capture, once, etc.)

### 3.3 L'objet `event`

Chaque fonction gestionnaire reçoit automatiquement un objet `event` contenant des informations utiles.

```javascript
document.querySelector("#monLien").addEventListener("click", function (event) {
  event.preventDefault(); // empêche le comportement par défaut (ex: suivre le lien)
  console.log("Cible cliquée :", event.target);
  console.log("Type d'événement :", event.type);
});
```

Propriétés courantes de `event` :
- `event.target` : l'élément qui a déclenché l'événement
- `event.type` : le type d'événement ("click", "keydown"...)
- `event.preventDefault()` : annule le comportement par défaut du navigateur
- `event.stopPropagation()` : empêche l'événement de remonter aux parents

### 3.4 Types d'événements courants

```javascript
// Souris
element.addEventListener("click", handler);
element.addEventListener("dblclick", handler);
element.addEventListener("mouseover", handler);
element.addEventListener("mouseout", handler);

// Clavier
document.addEventListener("keydown", (e) => {
  console.log("Touche pressée :", e.key);
});
document.addEventListener("keyup", handler);

// Formulaires
formulaire.addEventListener("submit", (e) => {
  e.preventDefault(); // empêche le rechargement de la page
  console.log("Formulaire envoyé");
});

input.addEventListener("input", (e) => {
  console.log("Valeur actuelle :", e.target.value);
});

input.addEventListener("change", handler); // déclenché après perte de focus

// Fenêtre / document
window.addEventListener("load", () => console.log("Page entièrement chargée"));
document.addEventListener("DOMContentLoaded", () => console.log("DOM prêt"));
window.addEventListener("resize", handler);
window.addEventListener("scroll", handler);
```

### 3.5 La propagation des événements (bubbling)

Quand un événement se produit sur un élément, il "remonte" ensuite vers ses parents (phase de *bubbling*).

```html
<div id="parent">
  <button id="enfant">Cliquez-moi</button>
</div>
```

```javascript
document.querySelector("#parent").addEventListener("click", () => {
  console.log("Clic détecté sur le parent");
});

document.querySelector("#enfant").addEventListener("click", () => {
  console.log("Clic détecté sur le bouton");
});

// Cliquer sur le bouton affiche les deux messages, dans cet ordre :
// "Clic détecté sur le bouton"
// "Clic détecté sur le parent"
```

Pour empêcher cette remontée : `event.stopPropagation()`.

### 3.6 La délégation d'événements

Technique très utile : au lieu d'attacher un écouteur à chaque enfant, on l'attache au parent et on vérifie la cible réelle (`event.target`). Pratique pour les listes dynamiques.

```javascript
const liste = document.querySelector("#maListe");

liste.addEventListener("click", function (event) {
  if (event.target.tagName === "LI") {
    console.log("Élément cliqué :", event.target.textContent);
  }
});

// Fonctionne même pour des <li> ajoutés dynamiquement après coup !
```

### 3.7 Supprimer un écouteur d'événement

```javascript
function direBonjour() {
  console.log("Bonjour !");
}

bouton.addEventListener("click", direBonjour);

// Plus tard :
bouton.removeEventListener("click", direBonjour);
```

⚠️ Il faut passer une **référence de fonction nommée** (pas une fonction anonyme) pour pouvoir la retirer ensuite.

---

## Exercice de synthèse

Voici un petit exemple qui combine boucles, DOM et événements : une liste de tâches simple.

```html
<input type="text" id="nouvelleTache" placeholder="Nouvelle tâche">
<button id="ajouter">Ajouter</button>
<ul id="listeTaches"></ul>
```

```javascript
const input = document.querySelector("#nouvelleTache");
const boutonAjouter = document.querySelector("#ajouter");
const liste = document.querySelector("#listeTaches");

boutonAjouter.addEventListener("click", function () {
  if (input.value.trim() === "") return;

  const li = document.createElement("li");
  li.textContent = input.value;

  // Bouton de suppression pour chaque tâche
  const supprimer = document.createElement("button");
  supprimer.textContent = "Supprimer";
  supprimer.addEventListener("click", () => li.remove());

  li.appendChild(supprimer);
  liste.appendChild(li);

  input.value = "";
});

// Délégation : afficher le nombre total de tâches à chaque clic dans la liste
liste.addEventListener("click", function () {
  const total = liste.querySelectorAll("li").length;
  console.log("Total de tâches restantes : " + total);
});
```

Cet exemple utilise : la création dynamique d'éléments (DOM), une boucle implicite via `querySelectorAll`, et deux types d'écouteurs d'événements (`click` direct et délégation).

---

## Pour aller plus loin

- MDN Web Docs (référence officielle) : événements, DOM, boucles
- Pratiquer en créant de petits projets : liste de tâches, calculatrice, galerie d'images filtrable
- Explorer les événements avancés : `dragstart`/`drop` (drag & drop), `IntersectionObserver` (détection de visibilité au scroll)
