# PAC 2021 – amélioration de la CSS de la vue Lecteur d’écran

## Objectif 🚀

Simplification du partage du rapport « Aperçu du lecteur d’écran » de l’outil de vérification de l’accessibilité de PDF [PAC 2021](https://pdfua.foundation/en/pdf-accessibility-checker-pac) (PDF Accessibility Checker), au-delà de sa seule consultation immédiate.

## Stratégie adoptée ✨

Les appels _url()_ aux images dans la feuille de style de l’aperçu « Lecteur d’écran » ont été convertis en [data URI](https://developer.mozilla.org/fr/docs/Web/HTTP/Basics_of_HTTP/Data_URLs), avec un [outil dédié](https://gist.github.com/3l3gant-cod3s/6d5bab4b8f5c116e7b447538a8095a62) en Python 3 après modification d’une [version originale de celui-ci](https://gist.github.com/jsocol/1089733) en Python 2. La feuille de style est basée sur _normalize.css_ de [necolas](https://github.com/necolas), toutes deux sont sous licence MIT, ce qui a permis l’adaptation.

Les images ont été au préalable recompressées avec [Oxipng](https://github.com/shssoichiro/oxipng) pour réduire leur taille sans modifier leur apparence de façon perceptible.

Si elles avaient été nombreuses elles auraient pu être vérifiées avec [PerceptualDiff](http://pdiff.sourceforge.net/) mais ne l’ont été que « manuellement ».

Par ailleurs le fichier _normalize.css_ a été refactorisé pour réduire la plupart des répétitions, notamment afin de limiter le nombre d’occurrences de chacune des data-URIs, assez verbeuses, à une seule.

La taille résultante est du coup plus faible que celle de l’ensemble initial (feuille de style CSS + images).

## Comment l’utiliser ? 🛠️

Après avoir ouvert l’aperçu « Lecteur d’écran », visualisez son code source HTML par le menu contextuel (touche dédiée du clavier ou clic-droit, généralement) avec l’item « Afficher la source » (comme dans la capture d’écran ci-dessous).

![capture d’écran décrite précédemment](https://github.com/3l3gant-cod3s/PAC-2021-CSS-Screenreader-View/blob/3l3gant-cod3s-PAC-2021-inlining-images/source-PAC3.png "PAC 2021 Screenshot")

Remplacer dans ce code HTML le lien &lt;link rel="stylesheet" type="text/css" href="(…).css"&gt; vers la feuille de style originale (qui elle-même pointe les images via des url(…)) par une paire de balise <style>…</style> et en insérant entre celles-ci le contenu de [normalize.css](https://github.com/3l3gant-cod3s/PAC-2021-CSS-Screenreader-View/blob/3l3gant-cod3s-PAC-2021-inlining-images/normalize.css).

Et Voilà! vous obtenez un aperçu « Lecteur d’écran » autoporteur que vous pouvez partager sans aléas.

Remarque : une vue textuelle généralement équivalente peut-être obtenue avec l’option _-struct-text_ de l’outil _pdfinfo_ des [_poppler-utils_ (en anglais)](https://en.wikipedia.org/wiki/Poppler_(software)#poppler-utils), en ligne de commande.

## À quoi ça sert ? 🤔

Ce rapport permet par exemple de vérifier que des images qui méritent un texte équivalent parce qu’elles sont porteuse de sens (un logo qui identifie un ministère dans une circulaire…) le possèdent bien. Que l’ordre de lecture est logique. Ou que la structure visuelle, apparente, est bien répercutée dans une structure formelle telle que des titres, des listes et sous-listes etc. Toutes ces informations pourront être exploitées par les aides techniques telles qu’une synthèse vocale ou encore un lecteur de PDF adaptatif, par exemple [VIP Reader](https://www.ucba.ch/moyens-auxiliaires/outils-numeriques/premier-lecteur-pdf-pour-personnes-malvoyantes).

🫶
