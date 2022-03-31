<h1>To-do-list</h1>

<p>Correction et amélioration d'une application de to-do list pour le projet 8 du parcours de développeur d'application Frontend - OpenClassrooms -.</p>

<p>Le projet comporte quelques bugs à identifier et corriger, quelques optimisations possibles à intégrer.</p>
 
<p>Les tests unitaires sont à améliorer en utilisant le framework Jasmine.</p>

<p>Un audit de performance est attendu sur un site concurrent à notre application, la mise en évidence de certaines fonctionnalités intéressantes est attendue.</p>

<p>Enfin une documentation utilisateur ainsi qu'une documentation technique sont attendues.</p>


<h2>Structure de l'application</h2>

<p>Arborescence représentant les principaux fichiers de l'application, suivi de leur déscription détaillé.</p>

```
📄 index.html

📁 js
↳ 📄 helpers.js
↳ 📄 app.js
↳ 📄 store.js
↳ 📄 model.js
↳ 📄 template.js
↳ 📄 view.js
↳ 📄 controller.js

📁 node_modules

📁 test
↳ 📄 ControllerSpec.js
↳ 📄 SpecRunner.html
```

<p>Vous trouverez plus d'information sur l'application dans le <a href="https://github.com/Jean-MarcG/ocr-p8/wiki">wiki</a>.

<h2>Etape 1 : Corrigez les bugs</h2>

<h3>bug 1 : faute de frappe dans controller.js</h3>

<p>Faute de frappe: ligne 96 controller.js : Controller.prototype.addItem à la place de Controller.prototype.adddItem</p>

```
Controller.prototype.addItem = function (title) {
	var self = this;

	if (title.trim() === '') {
		return;
	}

	self.model.create(title, function () {
		self.view.render('clearNewTodo');
		self._filter(true);
	});
};
```

<h3> bug 2 : création des ID dans store.js</h3>

<blockquote>
<p dir="auto">La méthode <a href="https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Objets_globaux/Date/now" rel="nofollow">Date.now()</a> est parfaitement adaptée. La fonction retourne un chiffre unique correspondant au nombre de millisecondes écoulées depuis le 1er Janvier 1970 00:00:00.</p>
</blockquote>

<blockquote>
<p dir="auto">Il s' agit donc de notre <strong>identifiant unique</strong>.</p>
</blockquote>

```
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
  
  <p>Les autres corrections et améliorations apportées au code sont consultables dans le <a href="https://github.com/Jean-MarcG/ocr-p8/wiki">wiki</a>.
  
  <h2>Etape 2 : où sont les tests ?!</h2>
  
  <p>Tests unitaires réalisés avec le framework<a href="https://github.com/jasmine/"> <strong>Jasmine</strong></a></p>
  
  <p>Pré-requis :</p>
  
  <ul>
    <li>installer <a href="https://www.npmjs.com/get-npm?utm_source=house&amp;utm_medium=homepage&amp;utm_campaign=free%20orgs&amp;utm_term=Install%20npm">NPM et NodeJs</a></li>
    <li>installer <a href="https://github.com/jasmine/jasmine/releases"> <strong>Jasmine</strong></a></li>
  </ul>
  
  <p>Télécharger <a href="https://github.com/Jean-MarcG/ocr-p8">le dossier</a> puis ouvrir dans votre navigateur le fichier <strong>SpecRunner.html</strong> que vous trouverez en suivant le chemin suivant <code>../P8/test/SpecRunner.html</code></p>
  
  <p>L'ensemble des tests est consultable dans le fichier <a href="https://github.com/Jean-MarcG/ocr-p8/blob/master/test/ControllerSpec.js"><strong>ControllerSpec.js</strong></a></p>
  
  <blockquote>
    <ol dir="auto">
      <li>#62 =&gt; test si on affiche bien model et view</li>
      <li>#92 =&gt; test la view quand on affiche les todos de l'onglet active</li>
      <li>#114 =&gt; test la view quand on affiche les todos de l'onglet completed</li>
      <li>#179 =&gt; test la view si "All" est surligné quand on a l' onglet par défaut</li>
      <li>#188 =&gt; test la view si "Active" est surligné quand on change pour l'onglet active</li>
      <li>#198 =&gt; test le model quand on bascule tous les états des todos vers terminé</li>
      <li>#213 =&gt; test la mise à jour de view</li>
      <li>#232=&gt; test le model en cas d' ajout d'un todo</li>
      <li>#281 =&gt; test le model si on supprime un todo</li>
    </ol>
</blockquote>

  <p>Les tests ajoutés au projet sont consultables dans le <a href="https://github.com/Jean-MarcG/ocr-p8/wiki">wiki</a>.


  
  
