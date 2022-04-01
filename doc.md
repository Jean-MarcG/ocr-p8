<p>Liste des différentes améliorations apportées au code de l'application :</p>

<ul>
    <li>
    <a href="#correction-des-bugs">Correction des bugs</a>
    <ul>
        <li><a href="#controllerjs---additem---faute-de-frappe"><code>controller.js - addItem()</code> - Faute de frappe</a></li>
        <li><a href="#storejs---save---cr%c3%a9ation-des-id"><code>store.js - save()</code> - Création des ID</a></li>
    </ul>
    </li>
<li>
<a href="#simplification-du-code">Simplification du code</a>
<ul>
<li>
<a href="#suppression-code-inutile">Suppression code inutile</a>
<ul>
<li>
<a href="#une-boucle-inutile">Une boucle inutile</a>
<ul>
<li><a href="#controllerjs---removeitem-157"><code>controller.js - removeItem()</code> #157</a></li>
</ul>
</li>
<li>
<a href="#des-conditions-am%c3%a9lior%c3%a9es">Des conditions améliorées</a>
<ul>
<li><a href="#controllerjs---additem-99"><code>controller.js - addItem()</code> #99</a></li>
<li><a href="#storejs---find-42"><code>store.js - find()</code> #42</a></li>
<li><a href="#viewjs---elementcomplete-48"><code>view.js - _elementComplete()</code> #48</a></li>
<li><a href="#viewjs---edititem-71"><code>view.js - _editItem()</code> #71</a></li>
<li><a href="#viewjs---edititemdone-101"><code>view.js - _editItemDone()</code> #101</a></li>
</ul>
</li>
</ul>
</li>
<li>
<a href="#refactorisation-de-la-m%c3%a9thode-bind">Refactorisation de la méthode <code>bind()</code></a>
<ul>
<li><a href="#viewjs---bind-214"><code>view.js - bind()</code> #214</a></li>
</ul>
</li>
</ul>
</li>
</ul>

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
<a id="user-content-controllerjs---additem-99" class="anchor" href="#controllerjs---additem-99"></a><code>controller.js - addItem()</code> #99</h5>
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

<p>Code amélioré :</p>

```javascript
addItem(title) {
    var self = this;

    if (title.trim() !== '') {
        self.model.create(title, function() {
            self.view.render('clearNewTodo');
            self._filter(true);
        });
    }
}

```

<h5>
<a id="user-content-storejs---find-42" class="anchor" href="#storejs---find-42"></a><code>store.js - find()</code> #42</h5>

<p>Code initial :</p>

```javascript
Store.prototype.find = function (query, callback) {
  if (!callback) {
    return;
  }

  var todos = JSON.parse(localStorage[this._dbName]).todos;

  callback.call(
    this,
    todos.filter(function (todo) {
      for (var q in query) {
        if (query[q] !== todo[q]) {
          return false;
        }
      }
      return true;
    })
  );
};
```

<p>Code amélioré :</p>

```javascript
find(query, callback) {
    if (callback) {
        var todos = JSON.parse(localStorage[this._dbName]).todos;
        callback.call(
            this,
            todos.filter(function(todo) {
                for (var q in query) {
                    if (query[q] !== todo[q]) {
                        return false;
                    }
                }
                return true;
            })
        );
    }
}
```

<h5>
<a id="user-content-viewjs---_elementcomplete-48" class="anchor" href="#viewjs---_elementcomplete-48"></a><code>view.js - _elementComplete()</code> #48</h5>

<p>Code initial :</p>

```javascript
View.prototype._elementComplete = function (id, completed) {
  var listItem = qs('[data-id="' + id + '"]');

  if (!listItem) {
    return;
  }

  listItem.className = completed ? "completed" : "";

  // In case it was toggled from an event and not by clicking the checkbox
  qs("input", listItem).checked = completed;
};
```

<p>Code amélioré :</p>

```javascript
_elementComplete(id, completed) {
    var listItem = qs('[data-id="' + id + '"]');

    if (listItem) {
        listItem.className = completed ? 'completed' : '';
        // On définit la tâche comme terminée par défaut
        qs('input', listItem).checked = completed;
    }
}
```

<h5>
<a id="user-content-viewjs---_edititem-71" class="anchor" href="#viewjs---_edititem-71"></a><code>view.js - _editItem()</code> #71</h5>

<p>Code initial :</p>

```javascript
View.prototype._editItem = function (id, title) {
  var listItem = qs('[data-id="' + id + '"]');

  if (!listItem) {
    return;
  }

  listItem.className = listItem.className + " editing";

  var input = document.createElement("input");
  input.className = "edit";

  listItem.appendChild(input);
  input.focus();
  input.value = title;
};
```

<p>Code amélioré :</p>

```javascript
_editItem(id, title) {
    var listItem = qs('[data-id="' + id + '"]');

    if (listItem) {
        listItem.className = listItem.className + ' editing';
        var input = document.createElement('input');
        input.className = 'edit';
        listItem.appendChild(input);
        input.focus();
        input.value = title;
    }
}
```

<h5>
<a id="user-content-viewjs---_edititemdone-101" class="anchor" href="#viewjs---_edititemdone-101"></a><code>view.js - _editItemDone()</code> #101</h5>

<p>Code initial :</p>

```javascript
View.prototype._editItemDone = function (id, title) {
  var listItem = qs('[data-id="' + id + '"]');

  if (!listItem) {
    return;
  }

  var input = qs("input.edit", listItem);
  listItem.removeChild(input);

  listItem.className = listItem.className.replace("editing", "");

  qsa("label", listItem).forEach(function (label) {
    label.textContent = title;
  });
};
```

<p>Code amélioré :</p>

```javascript
_editItemDone(id, title) {
    var listItem = qs('[data-id="' + id + '"]');

    if (listItem) {
        var input = qs('input.edit', listItem);
        listItem.removeChild(input);
        listItem.className = listItem.className.replace('editing', '');
        qsa('label', listItem).forEach(function(label) {
            label.textContent = title;
        });
    }
}
```

<h3>
<a id="user-content-refactorisation-de-la-méthode-bind" class="anchor" href="#refactorisation-de-la-m%C3%A9thode-bind"></a>Refactorisation de la méthode <code>bind()</code>
</h3>

<p>Nous avons remplacé les conditions <code>if else if</code> par un traitement <code>switch</code>. Ce choix à plusieurs avantages :</p>

<ul>
<li>Lisibilité du code</li>
<li>Maintenabilité du code</li>
<li>Performance du traitement</li>
</ul>

<p>A plus grande échelle, ce choix améliorerait surement les performances d'un projet.</p>

<h4>
<a id="user-content-viewjs---bind-214" class="anchor" href="#viewjs---bind-214"></a><code>view.js - bind()</code> #214</h4>

<p>Code initial :</p>

```javascript
View.prototype.bind = function (event, handler) {
  var self = this;
  if (event === "newTodo") {
    $on(self.$newTodo, "change", function () {
      handler(self.$newTodo.value);
    });
  } else if (event === "removeCompleted") {
    $on(self.$clearCompleted, "click", function () {
      handler();
    });
  } else if (event === "toggleAll") {
    $on(self.$toggleAll, "click", function () {
      handler({ completed: this.checked });
    });
  } else if (event === "itemEdit") {
    $delegate(self.$todoList, "li label", "dblclick", function () {
      handler({ id: self._itemId(this) });
    });
  } else if (event === "itemRemove") {
    $delegate(self.$todoList, ".destroy", "click", function () {
      handler({ id: self._itemId(this) });
    });
  } else if (event === "itemToggle") {
    $delegate(self.$todoList, ".toggle", "click", function () {
      handler({
        id: self._itemId(this),
        completed: this.checked,
      });
    });
  } else if (event === "itemEditDone") {
    self._bindItemEditDone(handler);
  } else if (event === "itemEditCancel") {
    self._bindItemEditCancel(handler);
  }
};
```

<p>Code amélioré :</p>

```javascript
bind(event, handler) {
    var self = this;
    // amélioration
    switch (event) {
        case 'newTodo':
            $on(self.$newTodo, 'change', function() {
                handler(self.$newTodo.value);
            });
            break;
        case 'removeCompleted':
            $on(self.$clearCompleted, 'click', function() {
                handler();
            });
            break;
        case 'toggleAll':
            $on(self.$toggleAll, 'click', function() {
                handler({ completed: this.checked });
            });
            break;
        case 'itemEdit':
            $delegate(self.$todoList, 'li label', 'dblclick', function() {
                handler({ id: self._itemId(this) });
            });
            break;
        case 'itemRemove':
            $delegate(self.$todoList, '.destroy', 'click', function() {
                handler({ id: self._itemId(this) });
            });
            break;
        case 'itemToggle':
            $delegate(self.$todoList, '.toggle', 'click', function() {
                handler({
                    id: self._itemId(this),
                    completed: this.checked,
                });
            });
            break;
        case 'itemEditDone':
            self._bindItemEditDone(handler);
            break;
        case 'itemEditCancel':
            self._bindItemEditCancel(handler);
            break;
    }
}
```
