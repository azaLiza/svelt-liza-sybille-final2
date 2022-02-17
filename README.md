# Projet Svelte
LP1A
Liza BOUMALI
Sybille MICALLEF 
## Installation

 - Installer les dépendances
```bash
npm install
```
 - Lancer le serveur
```bash
npm run dev
```
## Notes
Le composant `DatabaseDocuments` permet de synchroniser un tableau de documents à une base de données. Lorsque la BDD est mise à jour, le tableau l'est aussi, et lorsque le tableau est mis à jour, la BDD l'est aussi. Notez que la suppression ne se fait pas au moyen de la suppression d'un élément dans le tableau mais de l'ajout un attribut `_deleted` à `true` qui est interprété comme une suppression par PouchDB.
Le composant `DatabaseDocument` permet simplement de récupérer un document, sans pouvoir le mettre à jour, étant donné que ce n'était pas une nécessité que nous avions.
