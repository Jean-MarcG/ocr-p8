<p>L'application est basée sur le modèle Model View Controller (MVC).</p>

<ul>
<li>Le modèle (Model) contient les données à afficher. ex: SQL</li>
<li>La vue (View) contient la présentation de l'interface graphique. ex: HTML &amp; CSS</li>
<li>Le contrôleur (Controller) contient la logique concernant les actions effectuées par l'utilisateur. ex: JavaScript</li>
</ul>
<h1>
<a id="user-content-javascript" href="#javascript"></a>Javascript</h1>
<h2>
<a id="user-content-appjs" href="#appjs"></a>app.js</h2>
<p>Permet d'instancier l'application avec la classe <code>Todo</code>.
On y instancie dans les arguments de son constructeur les classes nécessaires à son fonctionnement.</p>
```javascript
function Todo(name) {
    this.storage = new app.Store(name);
    this.model = new app.Model(this.storage);
    this.template = new app.Template();
    this.view = new app.View(this.template);
    this.controller = new app.Controller(this.model, this.view);
}
```
<p><code>setView()</code>: Permet de changer la vue en fonction de l'adresse actuelle.</p>

```javascript
function setView() {
  todo.controller.setView(document.location.hash);
}
```

<h2>
<a id="user-content-helpersjs" href="#helpersjs"></a>helpers.js</h2>
<p>Aide pour la manipulation du DOM. Se substitue à l'utilisation d'une librairie.</p>
<p><code>window.qs</code>: Récupère les éléments html par leurs sélecteurs CSS. <code>qs</code> = Query Selector.</p>
<p><code>window.qsa</code>: Récupère tout les éléments html par leurs sélecteurs CSS. <code>qsa</code> = Query Selector All.</p>

```javascript
window.qs = function (selector, scope) {
  return (scope || document).querySelector(selector);
};

window.qsa = function (selector, scope) {
  return (scope || document).querySelectorAll(selector);
};
```

<p><code>window.$on</code>: Lie un élément voulu à addEventListener.</p>

```javascript
window.$on = function (target, type, callback, useCapture) {
  target.addEventListener(type, callback, !!useCapture);
};
```

<p><code>window.$delegate</code>: Lie un gestionnaire d'évenement pour tout les éléments correspondant au sélecteur actuel ou futur, basé sur un élément racine.</p>

```javascript
window.$delegate = function (target, selector, type, handler) {
  function dispatchEvent(event) {
    var targetElement = event.target;
    var potentialElements = window.qsa(selector, target);
    var hasMatch =
      Array.prototype.indexOf.call(potentialElements, targetElement) >= 0;

    if (hasMatch) {
      handler.call(targetElement, event);
    }
  }

  var useCapture = type === "blur" || type === "focus";

  window.$on(target, type, dispatchEvent, useCapture);
};
```

<p><code>window.$parent</code>: Trouver l'élément parent d'un élement html a partir d'un tag.</p>

```javascript
window.$parent = function (element, tagName) {
  if (!element.parentNode) {
    return;
  }
  if (element.parentNode.tagName.toLowerCase() === tagName.toLowerCase()) {
    return element.parentNode;
  }
  return window.$parent(element.parentNode, tagName);
};
```

<h2><a id="user-content-controllerjs" href="#controllerjs"></a>controller.js</h2>
<p>Modifie le modèle et la vue suivant les actions réalisées par l'utilisateur avec la classe <code>Controller</code>.</p>

```javascript
function Controller(model, view) {
  var self = this;
  self.model = model;
  self.view = view;

  self.view.bind("newTodo", function (title) {
    self.addItem(title);
  });

  self.view.bind("itemEdit", function (item) {
    self.editItem(item.id);
  });

  self.view.bind("itemEditDone", function (item) {
    self.editItemSave(item.id, item.title);
  });

  self.view.bind("itemEditCancel", function (item) {
    self.editItemCancel(item.id);
  });

  self.view.bind("itemRemove", function (item) {
    self.removeItem(item.id);
  });

  self.view.bind("itemToggle", function (item) {
    self.toggleComplete(item.id, item.completed);
  });

  self.view.bind("removeCompleted", function () {
    self.removeCompletedItems();
  });

  self.view.bind("toggleAll", function (status) {
    self.toggleAll(status.completed);
  });
}
```

<p><code>setView()</code>: Charge et initialise la vue.</p>

```javascript
Controller.prototype.setView = function (locationHash) {
  var route = locationHash.split("/")[1];
  var page = route || "";
  this._updateFilterState(page);
};
```

<p><code>showAll()</code>: Décoché au chargement. Récupère les tâches et les affiche dans la liste.</p>

```javascript
Controller.prototype.showAll = function () {
  var self = this;
  self.model.read(function (data) {
    self.view.render("showEntries", data);
  });
};
```

<p><code>showActive()</code>: Fournit toutes les tâches actives.</p>

```javascript
Controller.prototype.showActive = function () {
  var self = this;
  self.model.read({ completed: false }, function (data) {
    self.view.render("showEntries", data);
  });
};
```

<p><code>showCompleted()</code>: Fournit toutes les tâches complètes.</p>

```javascript
Controller.prototype.showCompleted = function () {
  var self = this;
  self.model.read({ completed: true }, function (data) {
    self.view.render("showEntries", data);
  });
};
```

<p><code>addItem()</code>: A utiliser chaque fois qu'on veut ajouter une tâche. A insérer dans l'objet voulu, changera le DOM et sauvegarde la nouvelle tâche.</p>

```javascript
Controller.prototype.addItem = function (title) {
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

<p><code>editItem()</code>: Déclenche l'édition d'une tâche.</p>

```javascript
Controller.prototype.editItem = function (id) {
  var self = this;
  self.model.read(id, function (data) {
    self.view.render("editItem", { id: id, title: data[0].title });
  });
};
```

<p><code>editItemSave()</code>: Valide la fin de l'édition de la tâche.</p>

```javascript
Controller.prototype.editItemSave = function (id, title) {
  var self = this;

  while (title[0] === " ") {
    title = title.slice(1);
  }

  while (title[title.length - 1] === " ") {
    title = title.slice(0, -1);
  }

  if (title.length !== 0) {
    self.model.update(id, { title: title }, function () {
      self.view.render("editItemDone", { id: id, title: title });
    });
  } else {
    self.removeItem(id);
  }
};
```

<p><code>editItemCancel()</code>: Annule l'édition de la tâche.</p>

```javascript
Controller.prototype.editItemCancel = function (id) {
  var self = this;
  self.model.read(id, function (data) {
    self.view.render("editItemDone", { id: id, title: data[0].title });
  });
};
```

<p><code>removeItem()</code>: Permet de supprimer la tâche du DOM et de storage par son ID.</p>

```javascript
Controller.prototype.removeItem = function (id) {
  var self = this;
  var items;
  self.model.read(function (data) {
    items = data;
  });

  self.model.remove(id, function () {
    self.view.render("removeItem", id);
  });

  self._filter();
};
```

<p><code>removeCompletedItems()</code>: Supprime toutes les tâches finies du DOM et de storage.</p>

```javascript
Controller.prototype.removeCompletedItems = function () {
  var self = this;
  self.model.read({ completed: true }, function (data) {
    data.forEach(function (item) {
      self.removeItem(item.id);
    });
  });

  self._filter();
};
```

<p><code>toggleComplete()</code>: Donne l'ID du modèle et une checkbox, puis met à jour la tâche dans l'espace de stockage en fonction de l'état de la checkbox.</p>

```javascript
Controller.prototype.toggleComplete = function (id, completed, silent) {
  var self = this;
  self.model.update(id, { completed: completed }, function () {
    self.view.render("elementComplete", {
      id: id,
      completed: completed,
    });
  });

  if (!silent) {
    self._filter();
  }
};
```

<p><code>toggleAll()</code>: Bascule toutes les checkboxs en état on / off. A passer dans l'objet voulu.</p>

```javascript
Controller.prototype.toggleAll = function (completed) {
  var self = this;
  self.model.read({ completed: !completed }, function (data) {
    data.forEach(function (item) {
      self.toggleComplete(item.id, completed, true);
    });
  });

  self._filter();
};
```

<p><code>_updateCount()</code>:Met à jour les endroits de la page qui changent en fonction du nombre de tâches restantes.</p>

```javascript
Controller.prototype._updateCount = function () {
  var self = this;
  self.model.getCount(function (todos) {
    self.view.render("updateElementCount", todos.active);
    self.view.render("clearCompletedButton", {
      completed: todos.completed,
      visible: todos.completed > 0,
    });

    self.view.render("toggleAll", { checked: todos.completed === todos.total });
    self.view.render("contentBlockVisibility", { visible: todos.total > 0 });
  });
};
```

<p><code>_filter()</code>: Refiltre la liste des tâches.</p>

```javascript
Controller.prototype._filter = function (force) {
  var activeRoute =
    this._activeRoute.charAt(0).toUpperCase() + this._activeRoute.substr(1);

  this._updateCount();

  if (
    force ||
    this._lastActiveRoute !== "All" ||
    this._lastActiveRoute !== activeRoute
  ) {
    this["show" + activeRoute]();
  }

  this._lastActiveRoute = activeRoute;
};
```

<p><code>_updateFilterState()</code>: Met à jour l'état du filtre sélectionné.</p>

```javascript
Controller.prototype._updateFilterState = function (currentPage) {
  this._activeRoute = currentPage;

  if (currentPage === "") {
    this._activeRoute = "All";
  }

  this._filter();

  this.view.render("setFilter", currentPage);
};
```

<h2>
<a id="user-content-modeljs" href="#modeljs"></a>model.js</h2>
<p>Permet de créer un nouvel objet <code>Model</code> et permettre la manipulation des informations dans <code>LocalStorage</code>.</p>

```javascript
function Model(storage) {
  this.storage = storage;
}
```

<p><code>create()</code>: Crée un nouvel modèle.</p>

```javascript
Model.prototype.create = function (title, callback) {
  title = title || "";
  callback = callback || function () {};

  var newItem = {
    title: title.trim(),
    completed: false,
  };

  this.storage.save(newItem, callback);
};
```

<p><code>read()</code>: Trouve et retourne un modéle dans l'espace de stockage. Ne retourne rien si aucune requête est spécifiée.</p>

```javascript
Model.prototype.read = function (query, callback) {
  var queryType = typeof query;
  callback = callback || function () {};

  if (queryType === "function") {
    callback = query;
    return this.storage.findAll(callback);
  } else if (queryType === "string" || queryType === "number") {
    query = parseInt(query, 10);
    this.storage.find({ id: query }, callback);
  } else {
    this.storage.find(query, callback);
  }
};
```

<p><code>update()</code>: met à jour une tâche.</p>

```javascript
Model.prototype.update = function (id, data, callback) {
  this.storage.save(data, callback, id);
};
```

<p><code>remove()</code>: enlever une tâches du stockage.</p>

```javascript
Model.prototype.remove = function (id, callback) {
  this.storage.remove(id, callback);
};
```

<p><code>removeAll()</code>: enlever tout les tâches du stockage.</p>

```javascript
Model.prototype.removeAll = function (callback) {
  this.storage.drop(callback);
};
```

<p><code>getCount()</code>: retourne le nombre de tâches.</p>

```javascript
Model.prototype.getCount = function (callback) {
  var todos = {
    active: 0,
    completed: 0,
    total: 0,
  };

  this.storage.findAll(function (data) {
    data.forEach(function (todo) {
      if (todo.completed) {
        todos.completed++;
      } else {
        todos.active++;
      }

      todos.total++;
    });
    callback(todos);
  });
};
```

<h2>
<a id="user-content-storejs" href="#storejs"></a>store.js</h2>
<p>Permet de créer une BDD temporaire avec la classe <code>Store</code> dans <code>LocalStorage</code>.</p>

```javascript
function Store(name, callback) {
  callback = callback || function () {};

  this._dbName = name;

  if (!localStorage[name]) {
    var data = {
      todos: [],
    };

    localStorage[name] = JSON.stringify(data);
  }

  callback.call(this, JSON.parse(localStorage[name]));
}
```

<p><code>find()</code>: Trouve les tâches a partir d'une requête.</p>

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

<p><code>findAll()</code>: Retourne toute les tâches de l'espace de stockage.</p>

```javascript
Store.prototype.findAll = function (callback) {
  callback = callback || function () {};
  callback.call(this, JSON.parse(localStorage[this._dbName]).todos);
};
```

<p><code>save()</code>: Sauvegarde une tâche dans la BDD.</p>

```javascript
Store.prototype.save = function (updateData, callback, id) {
  var data = JSON.parse(localStorage[this._dbName]);
  var todos = data.todos;

  callback = callback || function () {};

  var newId = Date.now();

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
    updateData.id = parseInt(newId);

    todos.push(updateData);
    localStorage[this._dbName] = JSON.stringify(data);
    callback.call(this, [updateData]);
  }
};
```

<p><code>remove()</code>: Supprime une tâche de la BDD suivant son ID.</p>

```javascript
Store.prototype.remove = function (id, callback) {
  var data = JSON.parse(localStorage[this._dbName]);
  var todos = data.todos;
  var todoId;

  for (var i = 0; i < todos.length; i++) {
    if (todos[i].id == id) {
      todoId = todos[i].id;
      todos.splice(i, 1);
    }
  }

  localStorage[this._dbName] = JSON.stringify(data);
  callback.call(this, todos);
};
```

<p><code>drop()</code>: Vide la BDD actuelle et crée une nouvelle BDD.</p>

```javascript
Store.prototype.drop = function (callback) {
  var data = { todos: [] };
  localStorage[this._dbName] = JSON.stringify(data);
  callback.call(this, data.todos);
};
```

<h2>
<a id="user-content-templatejs" href="#templatejs"></a>template.js</h2>
<p>Permet de créer le modèle par défaut avec la classe <code>Template</code>.</p>

```javascript
function Template() {
  this.defaultTemplate =
    '<li data-id="{{id}}" class="{{completed}}">' +
    '<div class="view">' +
    '<input class="toggle" type="checkbox" {{checked}}>' +
    "<label>{{title}}</label>" +
    '<button class="destroy"></button>' +
    "</div>" +
    "</li>";
}
```

<p><code>show()</code>: Crée les balises <code>&lt;li&gt;</code> de chaque tâches.</p>

```javascript
Template.prototype.show = function (data) {
  var i, l;
  var view = "";

  for (i = 0, l = data.length; i < l; i++) {
    var template = this.defaultTemplate;
    var completed = "";
    var checked = "";

    if (data[i].completed) {
      completed = "completed";
      checked = "checked";
    }

    template = template.replace("{{id}}", data[i].id);
    template = template.replace("{{title}}", escape(data[i].title));
    template = template.replace("{{completed}}", completed);
    template = template.replace("{{checked}}", checked);

    view = view + template;
  }

  return view;
};
```

<p><code>itemCounter()</code>: Compteur de tâches restantes.</p>

```javascript
Template.prototype.itemCounter = function (activeTodos) {
  var plural = activeTodos === 1 ? "" : "s";

  return "<strong>" + activeTodos + "</strong> item" + plural + " left";
};
```

<p><code>clearCompletedButton()</code>: Affiche 'Clear completed' si les tâches complétées sont supérieur à 0.</p>

```javascript
Template.prototype.clearCompletedButton = function (completedTodos) {
  if (completedTodos > 0) {
    return "Clear completed";
  } else {
    return "";
  }
};
```

<h2>
<a id="user-content-viewjs" href="#viewjs"></a>view.js</h2>
<p>Permet la manipulation du DOM et fait la liaison entre le model et le controller. On utilise dans les méthodes les selecteurs créer dans <code>helpers.js</code>.</p>

```javascript
function View(template) {
  this.template = template;

  this.ENTER_KEY = 13;
  this.ESCAPE_KEY = 27;

  this.$todoList = qs(".todo-list");
  this.$todoItemCounter = qs(".todo-count");
  this.$clearCompleted = qs(".clear-completed");
  this.$main = qs(".main");
  this.$footer = qs(".footer");
  this.$toggleAll = qs(".toggle-all");
  this.$newTodo = qs(".new-todo");
}
```

<p><code>_removeItem</code>: Supprime une tâche.</p>

```javascript
View.prototype._removeItem = function (id) {
  var elem = qs('[data-id="' + id + '"]');

  if (elem) {
    this.$todoList.removeChild(elem);
  }
};
```

<p><code>_clearCompletedButton</code>: Affiche ou non 'Clear completed'.</p>

```javascript
View.prototype._clearCompletedButton = function (completedCount, visible) {
  this.$clearCompleted.innerHTML =
    this.template.clearCompletedButton(completedCount);
  this.$clearCompleted.style.display = visible ? "block" : "none";
};
```

<p><code>_setFilter()</code>: Filtre les résultats suivant la page sélectionnée.</p>

```javascript
View.prototype._setFilter = function (currentPage) {
  qs(".filters .selected").className = "";
  qs('.filters [href="#/' + currentPage + '"]').className = "selected";
};
```

<p><code>elementComplete()</code>: Définit si une tâche est complète ou non.</p>

```javascript
View.prototype._elementComplete = function (id, completed) {
  var listItem = qs('[data-id="' + id + '"]');

  if (!listItem) {
    return;
  }

  listItem.className = completed ? "completed" : "";

  qs("input", listItem).checked = completed;
};
```

<p><code>_editItem()</code>: Affichage lors de l'édition d'une tâche.</p>

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

<p><code>_editItemDone()</code>: Affichage lors de la fin de l'édition d'une tâche.</p>

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

<p><code>render()</code>: Permet de gérer l'affichage avec les fonctions suivantes:</p>

```javascript
View.prototype.render = function (viewCmd, parameter) {
  var self = this;
  var viewCommands = {
    showEntries: function () {
      self.$todoList.innerHTML = self.template.show(parameter);
    },
    removeItem: function () {
      self._removeItem(parameter);
    },
    updateElementCount: function () {
      self.$todoItemCounter.innerHTML = self.template.itemCounter(parameter);
    },
    clearCompletedButton: function () {
      self._clearCompletedButton(parameter.completed, parameter.visible);
    },
    contentBlockVisibility: function () {
      self.$main.style.display = self.$footer.style.display = parameter.visible
        ? "block"
        : "none";
    },
    toggleAll: function () {
      self.$toggleAll.checked = parameter.checked;
    },
    setFilter: function () {
      self._setFilter(parameter);
    },
    clearNewTodo: function () {
      self.$newTodo.value = "";
    },
    elementComplete: function () {
      self._elementComplete(parameter.id, parameter.completed);
    },
    editItem: function () {
      self._editItem(parameter.id, parameter.title);
    },
    editItemDone: function () {
      self._editItemDone(parameter.id, parameter.title);
    },
  };

  viewCommands[viewCmd]();
};
```

<ul>
<li>
<p><code>showEntries</code>: Affiche les tâches.</p>
</li>
<li>
<p><code>removeItem</code>: Supprime une tâche.</p>
</li>
<li>
<p><code>updateElementCount</code>: Met à jour le nombre de tâches.</p>
</li>
<li>
<p><code>clearCompletedButton</code>: Met à jour l'affichage de 'Clear completed'.</p>
</li>
<li>
<p><code>contentBlockVisibility</code>: Affiche ou non le footer.</p>
</li>
<li>
<p><code>toggleAll</code>: Bascule toutes les tâches.</p>
</li>
<li>
<p><code>setFilter</code>: Affiche le filtre voulu.</p>
</li>
<li>
<p><code>clearNewTodo</code>: Réinitialise le champ de saisie principal.</p>
</li>
<li>
<p><code>elementComplete</code>: Affiche une tâche complète.</p>
</li>
<li>
<p><code>editItem</code>: Choisit une tâche en cours d'édition.</p>
</li>
<li>
<p><code>editItemDone</code>: Ferme une tâche en cours d'édition.</p>
</li>
</ul>
<p><code>_itemId()</code>: Retourne l'ID d'une tâche.</p>

```javascript
View.prototype._itemId = function (element) {
  var li = $parent(element, "li");
  return parseInt(li.dataset.id, 10);
};
```

<p><code>_bindItemEditDone()</code>: Gère l'arrêt d'édition d'une tâche.</p>

```javascript
View.prototype._bindItemEditDone = function (handler) {
  var self = this;
  $delegate(self.$todoList, "li .edit", "blur", function () {
    if (!this.dataset.iscanceled) {
      handler({
        id: self._itemId(this),
        title: this.value,
      });
    }
  });

  $delegate(self.$todoList, "li .edit", "keypress", function (event) {
    if (event.keyCode === self.ENTER_KEY) {
      this.blur();
    }
  });
};
```

<p><code>_bindItemEditCancel()</code>: Gère l'annulation d'édition d'une tâche.</p>

```javascript
View.prototype._bindItemEditCancel = function (handler) {
  var self = this;
  $delegate(self.$todoList, "li .edit", "keyup", function (event) {
    if (event.keyCode === self.ESCAPE_KEY) {
      this.dataset.iscanceled = true;
      this.blur();

      handler({ id: self._itemId(this) });
    }
  });
};
```

<p><code>bind()</code>: Gère le JavaScript suivant l'événement retourné.</p>

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

<h1><a id="user-content-css" href="#css"></a>CSS</h1>
<h2><a id="user-content-indexcss" href="#indexcss"></a>index.css</h2>
<p>Feuille de style de l'application.</p>

<h1><a id="user-content-node-modules" href="#node-modules"></a>NODE MODULES</h1>
<h2><a id="user-content-node-modules" href="#node-modules"></a>node_modules</h2>
<p>Contient le framework de test Jasmine et ses dépendances</p>

<h1><a id="user-content-test" href="#test"></a>TEST</h1>
<h2><a id="user-content-test" href="#test"></a>test</h2>
<p>Contient les tests Jasmine.</p>
<p><code>ControllerSpec.js </code>: Ce fichier contient les tests Jasmine pour chaque méthode que nous utilisons.</p>
<p><code>SpecRunner.html : </code>: Permet l'affichage html des résultats de tests Jasmine.</p>
