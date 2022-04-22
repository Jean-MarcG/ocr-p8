<h1>
<a id="user-content-audit-de-performance-en-comparaison-avec-lapplication-todolistmenet" href="#audit-de-performance-en-comparaison-avec-lapplication-todolistmenet"></a>Audit de performance en comparaison avec l'application <a href="http://todolistme.net/">todolistme.net</a>
</h1>
<p>Nous allons présenter dans cette partie un comparatif de performance entre l'application <a href="http://todolistme.net/">todolistme.net</a> et notre application <a href="http://to-do-list.folio-jmg.fr/" rel="nofollow">todos</a>.</p>
<p>Pour plus de fidélité au niveau des tests de performance, nous avons hébergé notre application sur un serveur en ligne (hébergeur Infomaniak).</p>
<p>Les tests de performances ont été effectués avec Google Lighthouse.</p>
<h3><a id="user-content-1--résumé-todolistme" href="#1--résumé-todolistme"></a>Résumé de l’ application <a href="http://todolistme.net/">todolistme.net</a></h3>
<p>Cette application permet de :</p>
<ol>
<li>créer des catégories de liste de tâches à effectuer</li>
<li>créer des liste de tâches à effectuer, les enregistrer, éditer, effacer</li>
<li>donner une temporalité</li>
</ol>
<p>Technologies utilisées sur le site :<p>
<img src="https://user-images.githubusercontent.com/56036510/164271788-4f3d864f-2741-4421-85b1-d0b4d86ac2e9.png" alt="technologies utilisées sur le site todolistme.net">
<h2>
<a id="user-content-résultats-google-lightouse" href="#r%C3%A9sultats-google-lightouse"></a>Résultats Google Lightouse</h2>

<table role="table">
<thead>
<tr>
<th>Todolistme.net</th>
<th>Notre application avant optimisation</th>
<th>Notre application après optimisation</th>
</tr>
</thead>
<tbody>
<tr>
<td><img src="https://user-images.githubusercontent.com/56036510/164272322-13f196a7-5666-4e56-8b1d-60b2d0a53a56.png" alt="Todolistme.net"></td>
<td><img src="https://user-images.githubusercontent.com/56036510/164748531-d4fbb72e-57ec-4585-bb1e-6a3be4eadd69.png" alt="Notre application avant optimisation"></td>
<td><img src="https://user-images.githubusercontent.com/56036510/164748650-7d12eb93-b874-4b0f-b9db-7b2f56a417c5.png" alt="Notre application après optimisation"></td>
</tr>
</tbody>
</table>
<h2>
<a id="user-content-performance" href="#performance"></a>Performances</h2>

<table role="table">
<thead>
<tr>
<th>Todolistme.net</th>
<th>Notre application après optimisation</th>
</tr>
</thead>
<tbody>
<tr>
<td><img src="https://user-images.githubusercontent.com/56036510/164052433-2142cf8d-6418-4bdd-93ff-85582090a6a2.png" alt="résultats tests performances lighthouse site todolistme"></td>
<td><img src="https://user-images.githubusercontent.com/56036510/164051876-52d71d87-ddf7-46a0-9bd1-e2b3339b82c6.png" alt="résultats tests performances lighthouse site todos après optimisation"></td>
</tr>
</tbody>
</table>
<h3>
<a id="user-content-todos" href="#todolistme"></a>Todos</h3>
<p>Suite à ces résultats, nous voyons que notre page se charge et est entièrement interactive 0.3s après son lancement.</p>
<p>C'est un très bon résultat qui s'explique principalement parce que notre code est entièrement optimisé en JavaScript vanilla.</p>

<h3>
<a id="user-content-todolistme" href="#todolistme"></a>Todolistme</h3>
<p>Suite à ces résultats, on peut se rendre compte que le contenu principal de la page est visible après 1.3s, c'est un bon score.</p>
<p>Cependant, de multiples améliorations pourraient être apportées : </p>
<ul>
<li>Utiliser du CSS au lieu d'une image pour l'image de fond</li>
<li>Utilisation d'un protocole HTTP/2</li>
<li>Utilisation de balises <meta name="viewport"></li>
</ul>
<h3>
<a id="user-content-conclusion-sur-les-performances" href="#conclusion-sur-les-performances"></a>Conclusion sur les performances</h3>
<p>Au niveau de la performance, notre application met 0.3s pour charger complétement et 1.3s pour todolistme.net.
Mais il ne faut pas oublier que todolistme.net offre plus de possibilités et charge des librairies en plus que nous n'utilisons pas ainsi que de la publicité..
<h3>
<a id="user-content-légende" href="#légende"></a>Légende</h3>
<p><code>First Contentful Paint</code> marque le temps à partir de quand le premier texte ou image est dessiné à l’écran.</p>
<p><code>Speed Index</code> correspond au délai moyen nécessaire à l'affichage des pixels au-dessus de la ligne de flottaison, soit la progression de l’affichage de la page.</p>
<p><code>Largest Contentful Paint </code> indique le temps de rendu du plus grand élément de contenu visible dans le viewport.</p>
<p><code>Time to interactive</code> indique le délai à partir duquel la page sera pleinement interactive.</p>
<p><code>Total Blocking Time</code> mesure le temps cumulé durant lequel une page est dans l'incapacité de répondre efficacement à une interaction utilisateur</p>
<p><code>Cumulative Layout Shift</code> permet d’évaluer la stabilité visuelle d’une page web.</p>

<h2>
<a id="user-content-accessibility" href="#accessibility"></a>Accessibilité</h2>
<table role="table">
<thead>
<tr>
<th>Todolistme.net</th>
<th>Notre application avant/après</th>
</tr>
</thead>
<tbody>
<tr>
<td><img src="https://user-images.githubusercontent.com/56036510/164275166-50d6be05-6bd3-4cf4-97d9-cb9c180de2e5.png" alt="Todolistme.net"></td>
<td>
<img src="https://user-images.githubusercontent.com/56036510/164282558-4999065e-40df-413b-bf52-f7781cb41f3d.png" alt="Notre application avant optimisation">
<br>
<img src="https://user-images.githubusercontent.com/56036510/164276813-248ba9bb-85f3-44ab-b211-d97b2dc0c95e.png" alt="Notre application après optimisation">
</td>
</tr>
</tbody>
</table>
<p>Au niveau de l'accessibilité, todolistme.met à beaucoup d'erreurs facilement corrigeable. En voici quelques-unes:</p>
<ul>
<li>Les images n'ont pas d'attributs => ajout d'une balise <code>alt</code>
</li>
<li>Les <code>input</code> n'ont pas de titre => ajout d'une balise <code>lablel</code></li>

<li>
Les éléments <code>frame</code> ou <code>iframe</code> n'ont pas de titre => ajout d'une balise <code>title</code>
</li>
</ul>
<p>Concernant notre application Todos, nous avons fait quelques modifications de notre css pour obtenir un contraste suffisant entre le premier plan et l'arrière plan.</p>

<h2>
<a id="user-content-best-practices" href="#best-practices"></a>Best Practices</h2>
<table role="table">
<thead>
<tr>
<th>Todolistme.net</th>
<th>Notre application avant/après</th>
</tr>
</thead>
<tbody>
<tr>
<td><img src="https://user-images.githubusercontent.com/56036510/164467251-a2a5a81c-1351-4633-8f5b-627e1fb2c6f2.png" alt="Todolistme.net"></td>
<td>
<img src="https://user-images.githubusercontent.com/56036510/164467572-605545c2-07ea-4c06-819b-a87800852401.png" alt="Notre application avant optimisation">
<br>
<img src="https://user-images.githubusercontent.com/56036510/164467767-f6b83205-19d0-4f4f-ae33-1963dbeca2df.png" alt="Notre application après optimisation">
</td>
</tr>
</tbody>
</table>
<p>L'application todolistme.net a un problème de sécurité car elle n'utilise pas le protocole HTTPS. De plus, l'utilisation de librairies Javascript <code>jQuery UI@1.12.1</code> et <code>jQuery@2.2.4</code> avec des failles de sécurité connues sont sources de problèmes si exploitées par des pirates informatiques.</p>
<br>
<p>De notre côté, seulement des erreurs mineures dans la console développeur. Tout a été régler en ajoutant un favicon(logo) sur le fichier HTML ainsi qu'un fichier learn.json.</p>
<h2>
<a id="user-content-seo" href="#seo"></a>SEO</h2>
<table role="table">
<thead>
<tr>
<th>Todolistme.net</th>
<th>Notre application avant/après</th>
</tr>
</thead>
<tbody>
<tr>
<td><img src="https://user-images.githubusercontent.com/56036510/164633369-ae9d5a05-8bd0-4c7e-9b47-aeda7143deca.png" alt="Todolistme.net"></td>
<td>
<img src="https://user-images.githubusercontent.com/56036510/164472188-b6942a58-ebb3-426b-8af2-b49d02e78745.png" alt="Notre application avant optimisation" >
<br>
<img src="https://user-images.githubusercontent.com/56036510/164471976-2b50656f-a0bb-4203-9e33-7d34b15d96b2.png" alt="Notre application après optimisation">
</td>
</tr>
</tbody>
</table>
<p>Pour ce qui est du SEO, todolistme.net à pas mal d'erreurs à corriger :</p>
<ul>
<li>Un problème d'adaptation aux format mobile est détecté => ajout de la metabalise <code>&lt;meta name="viewport"&gt;</code></li>
<li>Les liens ne peuvent pas être explorés => attribut <code>href</code> est absent sur les balises <code>a</code></li>
<li>Les images n'ont pas d'attributs => ajout d'une balise <code>alt</code></li>
</ul>
 <br>
<p>Pour notre application, l'ajout des metabalises <code>&lt;meta name="viewport"&gt;</code> et <code>&lt;meta name="description"&gt;</code> ont permis de passer d'une note de 80 à 100.</p>
<h2>
<a id="user-content-comparaison" href="#comparaison"></a>Comparaison mesures GTMETRIX</h2>
<img src="https://user-images.githubusercontent.com/56036510/164643274-625193f6-7b5d-45d9-9893-86c46ec1c878.png" alt="comparaison résultats todos et todolistme avec l'outil gt metrix">
<p>Il apparait rapidement que l'application Todos est plus performante car plus légère (moins de fonctionnalités).</p>
<p>Ce support présente un intérêt quant à la présence de publicités sur le site Todolistme qui vient altérer les performances de cette application.</p>
<h2>
<h2 id="user-content-conclusions">
<a id="user-content-conclusions" href="#conclusions"></a>Conclusions</h2>
<p>Nos résultats nous permettent de montrer que le site todolistme.net est beaucoup moins performant que notre application. Néanmoins, tous les résultats présentés ici sont à nuancer car la pub présente sur le site todolistme.net joue un rôle important sur la performance du site. Pour cela, nous avons réaliser deux tests sur le site todolistme.net : avec et sans pub (à l'aide d'un bloqueur de publicité).</p>
<p>Les résultats sont ici logiques : le chargement de la page est beaucoup plus rapide sur le site quand les pubs ne sont pas chargées.</p>
<p>Dans le cas d'une application légère comme la Todos, cela n'impacte pas forcément le chargement du DOM et de l'application. Néanmoins, sur une application complexe, cela peut s'avérer contraignant pour exécuter des requêtes en fonction du comportement des utilisateurs.</p>
<h3>
<a id="user-content-todolistmenet-sans-pub-vs-todos" href="#todolistmenet-sans-pub-vs-todos"></a>todolistme.net (sans pub) vs. todos</h3>
<p>Au vu de nos résultats, il était plus logique de comparer nos deux applications en bloquant la publicité sur todolistme.net, afin de se rapprocher au maximum du comportement actuel de notre application.</p>
<h4>
<a id="user-content-format-des-images" href="#format-des-images"></a>Format des images</h4>
<p>Le format des images n'est pas bon sur le site todolistme.net. Il faut faire attention à utiliser les bons formats d'image tel que PNG ou JPEG afin de bénéficier d'une meilleur compression des images, et donc d'optimiser le poids des ressources chargées.</p>
<h4><a id="user-content-traitements-des-sources-js-et-css" href="#traitements-des-sources-js-et-css" ></a>Traitements des sources js et css</h4>
<p>D'une application à l'autre, les sources js et css pourraient être optimisées. Pour commencer, le javascript et le css ne sont pas minifiés. Lors du passage en production d'une application, il est essentiel de minifier les fichiers javascript et css afin d'optimiser, une nouvelle fois, le poids des ressources chargées.</p>
<p>Concrètement la minification d'un fichier permet de supprimer les espaces et les sauts de ligne entre les lignes de code et réduire la longueur des variables. Le code devient incompréhensible, c'est pour cela qu'il faut seulement le faire lorsqu'une application est entièrement fonctionnelle.</p>
<h4>
<a id="user-content-https" href="#https"></a>https</h4>
<p>Un point faible des deux applications est qu'elle ne charge pas dans un canal sécurisé comme le https. Il n'est pas forcément nécessaire de passer toutes les applications et sites en https mais dans le cas où on traite et enregistre des todos qui peuvent comporter des données sensibles (mot de passe, email etc...), il est nécessaire de charger les ressources sur un canal sécurisé.</p>
<h4>
<a id="user-content-jquery" href="#jquery"></a>jQuery</h4>
<p>Sur l'application todolistme.net, la version de jQuery utilisé est obsolète et contient des failles de sécurité. C'est encore une fois contraignant par rapport à la gestion des données sensibles des utilisateurs.</p>
<p>Cela nous permet d'introduire dans cet audit le concept de gestion des paquets utilisé dans une application. Une fois en production, une application doit toujours être surveillée pour maintenir à jour les versions des librairies js ou css utilisées. C'est déjà le cas sur notre application, qui est construite avec la gestion des dépendances de NodeJS.</p>
<h4>
<a id="user-content-webpack-une-amélioration-" href="#webpack-une-am%C3%A9lioration-"></a>Webpack, une amélioration ?</h4>
<p>Une amélioration possible de notre application serait d'utilisé Webpack, le gestionnaire de module qui nous permettrait de rectifier plusieurs choses sur l'application :</p>
<ul>
<li>Minifier le JS et le CSS</li>
<li>Diminuer le nombre de requête dans le fichier index.html</li>
<li>Pouvoir utiliser le lazyLoading qui permet de charger seulement le code quand on en a besoin (augmentation des performances)</li>
<li>Avoir un système de cache plus efficace</li>
<li>Lors du développement : avoir une gestion des erreurs plus efficaces (couplé à nos tests unitaires)</li>
<li>Le code de l'application serait plus maintenable</li>
</ul>

<a id="user-content-todolistmenet--pub-vs-sans-pub" href="#todolistmenet--pub-vs-sans-pub"></a>todolistme.net : pub vs sans pub</h3>

<h2>
<a id="user-content-améliorations-possible-de-notre-application" href="améliorations-possible-de-notre-application"></a>Améliorations possible de notre application</h2>
<p>Notre application a quelques points qui peuvent encore être améliorés.</p>
<ul>
<li>La création d'une session pour que l'utilisateur puisse récupérer sa liste depuis un autre appareil</li>
<li>Créer un système permettant à l'utilisateur de mieux organiser sa liste en créant plusieurs listes différentes</li>
<li>Permettre de partager sa liste pour qu'elle soit visible par d'autres utilisateurs</li>
</ul>

/**_ sommaire _**/

<h3>
<a id="user-content-guide-utilisateur" href="#utilisation"></a><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Documentation-fonctionnelle#guide-utilisateur-application-to-do-list">Documentation fonctionnelle : guide utilisateur</a>
</h3>
<ul>
<li><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Documentation-fonctionnelle#ajouter">Ajout d'une tâche</a></li>
<li><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Documentation-fonctionnelle#editer">Editer une tâche</a></li>
<li><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Documentation-fonctionnelle#supprimer-une-t%C3%A2che">Supprimer une tâche</a></li>
<li><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Documentation-fonctionnelle#afficher-toutes-les-t%C3%A2ches-termin%C3%A9es">Afficher toutes les tâches terminées</a></li>
</ul>
<h3>
<a id="user-content-documentation-technique" href="#documentation-technique"></a><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Documentation-technique">Documentation technique</a>
</h3>
<h3>
<a id="user-content-améliorations--corrections-technique" href="#am%C3%A9liorations--corrections-technique"></a><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Am%C3%A9lioration-du-code">Améliorations &amp; corrections technique</a>
</h3>
<ul>
<li><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Am%C3%A9lioration-du-code#correction-des-bugs">Correction des bugs</a></li>
<li><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Am%C3%A9lioration-du-code#simplification-du-code">Améliorations et simplification du code</a></li>

</ul>
<h3>
<a id="user-content-audit-de-performance" href="#audit-de-performance"></a><a href="https://github.com/Jean-MarcG/ocr-p8/wiki/Audit-de-performance">Audit de performance</a>
</h3>
