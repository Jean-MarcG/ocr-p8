<p>Liste des différentes améliorations apportées au code de l'application :</p>

<h2><a href="#correction-des-bugs">Correction des bugs</a></h2>

<ul>
<li><a href="#controllerjs---additem---faute-de-frappe"><code>controller.js - addItem()</code></a>- Faute de frappe</li>
<li><a href="#storejs---save---création-des-id"><code>store.js - save()</code></a>- Création des ID</li>
<li><a href="#indexhtml---Correction du bug qui empêchait le 'Toggle All' au clic sur la flèche du champ de saisie"><code>index.html - id="toggle-all"</code></a>- Il manquait un id à la checkbox située à gauche du champ de saisie principal</li>
<li><a href="#assets---Ajout d'un favicon pour ne plus avoir d'erreur 404"><code>favicon.ico</code></a> - Les navigateurs cherchent par défaut un fichier favicon.ico à la racine du site si aucun favicon n'est mentionné dans les meta du site. Cela implique dont une erreur 404.</li>
</ul>
</li>

<a href="#simplification-du-code">Simplification du code</a>

<h2>
<a id="user-content-correction-des-bugs" href="#correction-des-bugs">Correction des bugs</a>
</h2>

<h3><a id="user-content-controllerjs---additem---faute-de-frappe" href="#controllerjs---additem---faute-de-frappe"></a><code>controller.js - addItem()</code> - Faute de frappe</h3>

<p>Le premier bug corrigé correspondait à une faute de frappe sur le nom de la méthode <code>addItem()</code> (<code>adddItem()</code>). Ce bug empêchait tout simplement la création de nouvelles tâches.</p>

```javascript
Controller.prototype.addItem = function (title) {
  // avant : adddItem
  var self = this;

  if (title.trim() === "") {
    return;
  }

  self.model.create(title, function () {
    self.view.render("clearNewTodo");
    self._filter(true);
  });
};
```

<h3>
<a id="user-content-storejs---save---création-des-id" class="anchor" href="#storejs---save---cr%C3%A9ation-des-id"></a><code>store.js - save()</code> - Création des ID</h3>

<p>La création des ID n'est pas un bug mais pourrait le devenir car la façon de créer l'identifiant ne le rend pas unique. Et si deux identifiants étaient amenés à être identique, il y aurait un bug.</p>

<p>Pour corriger celà, il est possible d'utiliser une méthode basée sur la génération de date. On utilisera ici la méthode <code>Date.now()</code> qui renvoie le nombre de millisecondes écoulées depuis le 1er Janvier 1970. Cet identifiant est donc unique !</p>

<p>Voici la méthode save, améliorée :</p>

```javascript
Store.prototype.save = function (updateData, callback, id) {
  var data = JSON.parse(localStorage[this._dbName]);
  var todos = data.todos;

  callback = callback || function () {};

  // Generate an ID
  var newId = Date.now();
  // var charset = "0123456789";

  // for (var i = 0; i < 6; i++) {
  // 	newId += charset.charAt(Math.floor(Math.random() * charset.length));
  // }

  // If an ID was actually given, find the item and update each property
  if (id) {
    for (var i = 0; i < todos.length; i++) {
      if (todos[i].id === id) {
        for (var key in updateData) {
          todos[i][key] = updateData[key];
        }
        break;
      }
    }

    localStorage[this._dbName] = JSON.stringify(data);
    callback.call(this, todos);
  } else {
    // Assign an ID
    updateData.id = parseInt(newId);

    todos.push(updateData);
    localStorage[this._dbName] = JSON.stringify(data);
    callback.call(this, [updateData]);
  }
};
```

<h2><a id="user-content-simplification-du-code" class="anchor" href="#simplification-du-code"></a>Simplification du code</h2>

<h3><a id="user-content-suppression-code-inutile" class="anchor" href="#suppression-code-inutile"></a>Suppression code inutile</h3>
<p>Nous avons ici fait des améliorations au niveau de la synthaxe du code. Dans tous les cas, le projet fonctionne mais nos changements améliorent la qualité du code.</p>
<h4><a id="user-content-des-conditions-améliorées" class="anchor" href="#des-conditions-améliorées"></a>Des conditions améliorées</h4>

<p>L'amélioration ici a été de supprimer la boucle <code>forEach</code> qui servait simplement à afficher un console.log de l'élément supprimé.</p>
<p>Au lieu de ça, nous avons ajouté ce console.log directement dans la méthode remove du modèle. Grâce à cette amélioration, nous économisons une boucle et un traitement inutile.</p>
<h5>
<a id="user-content-controllerjs---removeitem-157" class="anchor" href="#controllerjs---removeitem-157"></a><code>controller.js - removeItem()</code> #157</h5>
<p>Code initial :</p>

```javascript
Controller.prototype.removeItem = function (id) {
  var self = this;
  var items;
  self.model.read(function (data) {
    items = data;
  });

  items.forEach(function (item) {
    if (item.id === id) {
      console.log("Element with ID: " + id + " has been removed.");
    }
  });

  self.model.remove(id, function () {
    self.view.render("removeItem", id);
  });

  self._filter();
};
```

<p>Code amélioré :</p>

```javascript
removeItem(id) {
    var self = this;
    var items;
    self.model.read(function(data) {
        items = data;
    });

    self.model.remove(id, function() {
        self.view.render('removeItem', id);
        console.log('Element with ID: ' + id + ' has been removed.');
    });
    self._filter();
}

```

<h4>
<a id="user-content-des-conditions-améliorées" class="anchor" href="#des-conditions-améliorées"></a>Des conditions améliorées</h4>
<p>Les 5 cas suivant présentent une amélioration de la syntaxe du code. Au lieu d'utiliser une condition pour effectuer un simple <code>return</code>, nous allons utiliser une condition dans tous le code. C'est une manière plus logique de faire.</p>
<h5>
<a id="user-content-controllerjs---additem-86" class="anchor" href="#controllerjs---additem-86"></a><code>controller.js - addItem()</code> #86</h5>
<p>Code initial :</p>

```javascript
Controller.prototype.adddItem = function (title) {
  var self = this;

  if (title.trim() === "") {
    return;
  }

  self.model.create(title, function () {
    self.view.render("clearNewTodo");
    self._filter(true);
  });
};
```
