---
layout:     post
title:      "Setup up a jekyll blog"
subtitle:   "An easy way to get a good looking blog using jekyll and github pages"
date:       2014-10-19 12:00:00
author:     "Vincent Daubry"
header-img: "img/post-bg-02.jpg"
---

###Mettre en ligne un blog perso avec Jekyll

TL;DR :

Vous êtes développeur ? Vous avez envie de démarrer un blog perso ? Vous voulez faire simple et avoir un résultat sympa visuellement ?
Dans cet article nous allons voir comment mettre en place un blog avec <a href="http://jekyllrb.com/">jekyll</a>, utiliser un thème sympa, et héberger le tout sur <a href="https://pages.github.com/">github pages</a>


C'est le 1er article de mon blog et pour commencer par une vertigineuse mise en abime, on va voir comment j'ai mis en place ce blog. Ca vous donne tout de suite une idée du résultat finale, si ça ne vous convient pas vous pouvez passer votre chemin.


###Pourquoi un générateur de site statique ?

Il y a plusieurs raisons de choisir un générateur de site statique plutôt qu'un CMS du type wordpress. Dans cet article on va se concentrer sur la simplicité apportée par un générateur de site statique :

* Prise en main rapide (si vous connaissez markdown et html ça devrait aller)
* Très peu de configuration
* Facilité de déploiement, il n'y a pas de serveur d'app
* Possibilité d'héberger directement sur Github grace à <a href="https://pages.github.com/">github pages</a>

La liste des solutions est <a href="https://staticsitegenerators.net">impressionnante</a>, alors pourquoi Jekyll ?

* C'est un des plus répandu selon <a href="https://www.staticgen.com/">ce classement</a>
* Utilisé par Github (écrit par Tom Preston-Werner, fondateur de Github)
* L'écosystème Ruby


###Installer Jekyll à partir d'un thème

Le template par défaut de Jekyll est pas terrible en terme de design. Si vous allez sur <a href="http://jekyllthemes.org/">jekyllthemes.org</a>, vous trouverez des templates plus sympa. Pour ce blog je suis parti du thème <a href="https://github.com/IronSummitMedia/startbootstrap-clean-blog-jekyll">clean-blog</a>

Pour démarrer votre blog vous pouvez simplement forker le repo. Pour pré-visualiser les article il faut installer Jekyll :

```gem install jekyll```


###Deployer votre blog Jekyll

Il suffit de générer la version statique de votre blog :

```jekyll build ```

Vous pouvez ensuite servir ce site statique comme vous voulez :

* Depuis votre serveur
* Depuis un bucket S3
etc

Vous pouvez également utiliser le service d'hébergement de github pages, pour ça il suffit de créer un repo en utilisant la convention suivante pour le nom du repo :

```
your_github_username.github.io
```

A chaque commit sur master Github va automagiquement builder une version statique de votre site et la déployer.


###Ajouter des commentaires à votre blog jekyll avec disqus

Pour ajouter la possibiliter de laisser des commentaires avec disqus, c'est tout aussi simple :

* Créer un compte sur <a href="https://disqus.com">disqus</a>
* Choisissez l'intégration via "universal code"
* Copier le code fournit dans le layout des post (```_layout/post.hmtl```)

C'est tout.


###Ajouter des liens de partage sur twitter, google+ et facebook

Pour ajouter des liens de partage sur vos posts, la procédure est extrèmement simple pour twitter et google+ , il suffit de coller le lien dans votre page

* Twitter : Pour ajouter un lien de partage twitter, rendez vous sur <a href="https://about.twitter.com/resources/buttons#tweet">cette page</a>
* Google + : Pour ajouter un lien de partage twitter, rendez vous sur <a href="https://developers.google.com/+/web/share/">cette page</a>

Pour facebook il est nécessaire de créer une application, pour cela rendez vous sur : https://developers.facebook.com/
Puis rendez vous sur la page du bouton partage pour obtenir le code à ajouter sur votre page : https://developers.facebook.com/docs/plugins/share-button/

Décalage de pixels bouton partage de facebook : ajouter : position: relative; top: -8px; left: 33px;



###Add links to share on twitter, facebook and google +

To add sharing links on your posts, the procedure is extremely simple for twitter and google +, just paste the provided code on your page :

* Twitter: To add a link to twitter sharing, visit <a href="https://about.twitter.com/resources/buttons#tweet">this page</a>
* Google +: To add a link sharing twitter, visit <a href="https://developers.google.com/+/web/share/">this page</a>

For Facebook c'est plus galère : il est nécessaire de créer une application, pour cela rendez vous sur la page <a href="https://developers.facebook.com/">facebook developer</a>

Puis rendez vous sur <a href="https://developers.facebook.com/docs/plugins/share-button/">la page du bouton partage</a> pour obtenir le code à ajouter sur votre page

Facebook inject le style pour son bouton via du javascript. Cela peut provoquer un décalage d'alignement vertical avec les autres boutons :

<img src="/img/posts/2014-11-19-setup-a-jekyll-blog/facebook-bug.png" height="90">

Dans mon cas j'ai résolu le problème avec un gros bout de sparadra :

{% highlight html %}
<div class="fb-share-button" data-href="{{site.url}}{{page.url}}" data-layout="button_count" style="position: relative; top: -8px; left: 33px;"></div>
{% endhighlight %}

Les developper front end doivent avoir les yeux qui brulent en lisant ça. Si vous avez une solution plus clean les commentaires sont là pour ça.

Attention: Le code fournit par facebook référence la page facebook du bouton partage (non vous n'avez pas 1 million de partage sur votre article de blog).
Vous pouvez utiliser les <a href="http://jekyllrb.com/docs/variables/">variables Jekyll</a> pour obtenir l'url de la page courrante :

```
{{site.url}}{{page.url}}
```


###Ajouter des meta pour google, facebook and twitter

Les moteurs de recherches et les réseaux sociaux crawl voter page et l'index ses metadats, dans le cas de google cela participe au calcul de votre <a href="http://en.wikipedia.org/wiki/PageRank">pagerank</a>, dans le cas de Facebook (meta og) et Twitter (twitter cards) cela permet d'enrichir l'affichage de votre page dans les partages et les tweets.

Le sujet est vaste et je vous recommende cet excellent article sur l'état de l'art des meta en 2013 : <a href="http://www.iacquire.com/blog/18-meta-tags-every-webpage-should-have-in-2013">18 Meta Tags Every Webpage Should Have in 2013</a>

Nous allons simplement voir dans cet article comment ajouter ces meta dans la balise head de toutes les pages de jekyll.



####Metadatas Facebook OpenGraph :


Partager un lien sur Facebook consiste à référencer sur sa timeline un objet de <a href="http://en.wikipedia.org/wiki/Facebook_Platform#Open_Graph_protocol">l'open graph de facebook</a>

Facebook crawl le web et ajoute chaque page en tant qu'objet de son graph. Par défaut le crawler va utiliser les informations dont il dispose comme le titre de votre page.

Vous pouvez améliorer le référencement de votre page dans l'open graph en fournissant au crawler les informations dont il a besoin :

* titre du site
* titre du post
* url nettoyée (utilisé comme identifiant unique)
* description
* image (thumbnail)
* Facebook app id
* type
* language
* author

Vous devez ajouter ces tags dans la balise ```<head>``` de chaque page du site, il faut donc prévoir le cas des pages qui sont des posts et le cas des autres pages du site (homepage, about, etc)

Pour celà vous pouvez ajouter un code de type dans votre include ```head.html``` :

{% gist 69f089affeff3b6ca627 %}

N'oubliez pas de vérifier la bonne intégration des metadatas open graph sur le <a href="https://developers.facebook.com/tools/debug/og/object/">debugger facebook</a>

Si vous souhaitez une liste des attributs de l'open graph vous pouvez lire la doc <a href="http://ogp.me/">ici</a>


####Metadatas Twitter cards :

Le concept est similaire pour Twitter, pour afficher un appercu d'un lien dans un tweet vous pouvez fournir au crawler de Twitter des informations sur votre page :

* card : Texte, image, ou player (ex: video)
* url
* titre
* description
* image


Voilà un exemple de code que vous pouvez ajouter dans votre include ```head.html``` pour générer les metadatas Twitter cards :

{% gist d404f792a87699c665e2 %}

Comme pour Facebook pensez à valider l'intégration des metadatas Twitter cards via le <a href="https://cards-dev.twitter.com/validator">Card validator</a>

Pour que twitter affichent les images fournit il faut que votre domaine soit validé par Twitter. Pour celà rendez vous sur le <a href="https://cards-dev.twitter.com/validator">Card validator</a> et cliquez sur "request approval".

Pour ce blog la validation a été faite quelques minutes après avoir fait la demande.


###SEO

Je ne suis pas expert SEO et ce n'est pas le sujet de cet article, cela étant dit il y a t'il des choses simple que l'on peut faire pour améliorer le référencement de ce blog ?

Voilà la liste des modifications que j'ai apporté au template clean blog :

1) Ne pas inclure systématiquement le nom du blog dans la balise title

source : <a href="http://sixrevisions.com/content-strategy/5-common-seo-mistakes-with-web-page-titles/)">5 Common SEO Mistakes with Web Page Titles</a>

Avant :

{% highlight html %}
{% raw %}
<title>{% if page.title %}{{ page.title }} - {{ site.title }}{% else %}{{ site.title }}{% endif %}</title>
{% endraw %}
{% endhighlight %}

Après :
{% highlight html %}
{% raw %}
<title>{% if page.title %}{{ page.title }}{% else %}{{ site.title }}{% endif %}</title>
{% endraw %}
{% endhighlight %}


2) Utiliser une meta description spécifique pour chaque page du blog

source : <a href="http://moz.com/learn/seo/meta-description">Meta Description Tag - Learn SEO</a>

Avant :

{% highlight html %}
{% raw %}
<meta name="description" content="{{ site.description }}">
{% endraw %}
{% endhighlight %}

Après :
{% highlight html %}
{% raw %}
<meta name="description" content="{% if page.description %}{{ page.description }}{% else %}{{ site.description }}{% endif %}">
{% endraw %}
{% endhighlight %}


3) Utiliser la meta rel=author pour chaque page du blog

source: <a href="http://www.vervesearch.com/blog/how-to-implement-the-relauthor-tag-a-step-by-step-guide/">How to Implement the Rel=”Author” Tag – A Step by Step Guide</a>

{% highlight html %}
<link rel="author" href="https://plus.google.com/+vincentdaubry"/>
{% endhighlight %}

Voyez vous d'autres améliorations que l'on peut apporter au référencement d'un blog Jekyll ?



Le code de ce blog est accessible ici : <a href="vdaubry.github.io">vdaubry.github.io</a>