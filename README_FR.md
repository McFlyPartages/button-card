# Button Card by [@RomRider](https://github.com/RomRider) <!-- omit in toc -->

[![Stable][releases-shield]][releases] [![Beta][releases-dev-shield]][releases-dev] [![HACS Badge][hacs-badge]][hacs-link] ![Project Maintenance][maintenance-shield] <br/> ![Downloads][downloads] [![GitHub Activity][commits-shield]][commits] [![License][license-shield]](LICENSE.md) [![Discord][discord-shield]][discord] [![Community Forum][forum-shield]][forum]

[commits-shield]: https://img.shields.io/github/commit-activity/y/custom-cards/button-card.svg
[commits]: https://github.com/custom-cards/button-card/commits/master
[discord]: https://discord.gg/Qa5fW2R
[discord-shield]: https://img.shields.io/discord/330944238910963714.svg
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen.svg
[forum]: https://community.home-assistant.io/t/lovelace-button-card/65981
[license-shield]: https://img.shields.io/github/license/custom-cards/button-card.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2023.svg
[releases-shield]: https://img.shields.io/github/release/custom-cards/button-card.svg
[releases]: https://github.com/custom-cards/button-card/releases/latest
[releases-dev-shield]: https://img.shields.io/github/package-json/v/custom-cards/button-card/dev?label=release%40dev
[releases-dev]: https://github.com/custom-cards/button-card/releases
[hacs-badge]: https://img.shields.io/badge/HACS-Default-41BDF5.svg
[downloads]: https://img.shields.io/github/downloads/custom-cards/button-card/total
[hacs-link]: https://hacs.xyz/

Cartes Lovelace Button-card pour vos entités.

![all](examples/all.gif)

## TOC <!-- omit in toc -->

- [Features](#features)
- [Configuration](#configuration)
  - [Main Options](#main-options)
  - [Action](#action)
  - [Confirmation](#confirmation)
  - [Lock Object](#lock-object)
  - [State](#state)
  - [Available operators](#available-operators)
  - [Layout](#layout)
  - [`triggers_update`](#triggers_update)
  - [Javascript Templates](#javascript-templates)
  - [Styles](#styles)
    - [Easy styling options](#easy-styling-options)
    - [Light entity color variable](#light-entity-color-variable)
    - [ADVANCED styling options](#advanced-styling-options)
    - [Injecting CSS with `extra_styles`](#injecting-css-with-extra_styles)
  - [Custom Fields](#custom-fields)
  - [Configuration Templates](#configuration-templates)
    - [General](#general)
    - [Merging state by id](#merging-state-by-id)
    - [Variables](#variables)
- [Installation](#installation)
  - [Manual Installation](#manual-installation)
  - [Installation and tracking with `hacs`](#installation-and-tracking-with-hacs)
- [Examples](#examples)
  - [Configuration with states](#configuration-with-states)
    - [Default behavior](#default-behavior)
    - [With Operator on state](#with-operator-on-state)
    - [`tap_action` Navigate](#tap_action-navigate)
    - [blink](#blink)
  - [Play with width, height and icon size](#play-with-width-height-and-icon-size)
  - [Templates Support](#templates-support)
    - [Playing with label templates](#playing-with-label-templates)
    - [State Templates](#state-templates)
  - [Styling](#styling)
  - [Lock](#lock)
  - [Aspect Ratio](#aspect-ratio)
  - [Changing the feedback color during a click](#changing-the-feedback-color-during-a-click)
- [Community guides](#community-guides)
- [Credits](#credits)

## Features

- Fonctionne avec n'importe quelle entité basculante
- 6 actions disponibles sur **tap** et/ou **hold** et/ou **double click** : `none`, `toggle`, `more-info`, `navigate`, `url`, `assist` et `call-service`.
- Affichage de l'état (optionnel)
- Couleur personnalisée (optionnelle), ou basée sur la valeur/température rgb de la lumière
- définition d'un état personnalisé avec une couleur, une icône et un style personnalisables (optionnel)
- [taille personnalisée de l'icône, largeur et hauteur](#Play-with-width-height-and-icon-size) (optionel)
- [prise en charge du ratio](#aspect-ratio) (optionel)
- Prise en charge des [modèles javascript](#javascript-templates) dans certains champs
- Icône personnalisée (optionnel)
- Style css personnalisé (optionnel)
- Prise en charge de plusieurs [layout](#Layout) et [layout personnalisée](#advanced-styling-options)
- Les unités pour les capteurs peuvent être redéfinies ou cachées
- 2 types de couleurs
  - `icon` : applique les paramètres de couleur à l'icône uniquement
  - `card` : applique les paramètres de couleur à la carte uniquement
- couleur de police automatique si `color_type` est réglé sur `card`
- blank card et label card (pour l'organisation)
- support de l'animation [blink](#blink)
- upport d'animation rotative
- Fenêtre de confirmation pour les éléments sensibles (optionnel) ou [mécanisme de verrouillage](#lock)
- support haptique pour l'[application compagnon IOS](https://companion.home-assistant.io/docs/integrations/haptics)
- prise en charge d [custom_updater](https://github.com/custom-components/custom_updater) et [HACS](https://github.com/hacs/integration)

## Configuration

### Main Options

| Name | Type | Default | Supported options | Description |
| --- | --- | --- | --- | --- |
| `type` | string | **Required** | `custom:button-card` | Type of the card |
| `template` | string | optional | any valid template from `button_card_templates`  | Voir [configuration template](#Configuration-Templates) |
| `entity` | string | optional | `switch.ac` | entity_id |
| `triggers_update` | string or array | optional | `switch.ac` | entity_id liste qui déclencherait une mise à jour de la carte, voir [triggers_update](#triggers_update) |
| `group_expand` | boolean | false | `true` \| `false` | Lorsque `true`, si l'une des entités déclenchant la mise à jour d'une carte est un groupe, le groupe s'agrandira automatiquement et la carte sera mise à jour lors de tout changement d'état de l'entité enfant. Cela fonctionne également avec les groupes imbriqués. Voir [triggers_update](#triggers_update) |
| `icon` | string | optional | `mdi:air-conditioner` | Icône à afficher. Elle sera remplacée par l'icône définie dans un état (si elle existe). La valeur par défaut est l'icône de l'entité. Cacher avec `show_icon : false`. Supporte les modèles, voir [templates](#javascript-templates) |
| `color_type` | string | `icon` | `icon` \| `card` \| `blank-card` \| `label-card` | Colorier soit le fond de la carte, soit l'icône à l'intérieur de la carte. Si l'on choisit `card`, la couleur de la police et de l'icône est automatique. Cela permet au texte/icône d'être lisible même si la couleur de fond est claire/foncée. Des options supplémentaires de type couleur `blank-card` et `label-card` peuvent être utilisées pour l'organisation (voir les exemples). |
| `color` | string | optional | `auto` \| `auto-no-temperature` \| `rgb(28, 128, 199)` | Couleur de l'icône/carte. `auto` définit la couleur en fonction de la couleur d'une lumière, y compris la température de la lumière. En mettant `auto-no-temperature`, la couleur se comportera comme la couleur par défaut de home-assistant, en ignorant la température de la lumière. Par défaut, si l'état de l'entité est `off`, la couleur sera `var(--paper-item-icon-color)`, pour `on` ce sera `var(--paper-item-icon-active-color)` et pour tout autre état ce sera `var(--primary-text-color)`. Vous pouvez redéfinir chaque couleur en utilisant `state`|
| `size` | string | `40%` | `20px` | Taille de l'icône. Peut être un pourcentage ou un pixel.|
| `aspect_ratio` | string | optional | `1/1`, `2/1`, `1/1.5`, ... | Voir [ici](#aspect-ratio) pour un exemple. Rapport d'aspect de la carte. `1/1` étant un carré. Ceci s'adaptera automatiquement à la taille de votre écran. |
| `tap_action` | object | optional | Voir [Action](#Action) | Définit le type d'action sur le clic, si non défini, la bascule sera utilisée. |
| `hold_action` | object | optional | Voir [Action](#Action) | Définir le type d'action lors de la mise en attente, si non défini, rien ne se passe. |
| `double_tap_action` | object | optional | Voir [Action](#Action) | Définit le type d'action lors d'un double clic, si non défini, rien ne se passe. |
| `name` | string | optional | `Air conditioner` | Définir un texte optionnel à afficher sous l'icône. Supporte les modèles, voir [templates](#javascript-templates).
| `state_display` | string | optional | `On` | Surcharge la façon dont l'état est affiché. Supporte les modèles, voir [templates](#javascript-templates). |
| `label` | string | optional | N'importe quelle chaîne que vous voulez | Affiche un label sous la carte. Voir [Layouts](#layout) pour plus d'informations. Supporte les modèles, voir [templates](#javascript-templates). |
| `show_name` | boolean | `true` | `true` \| `false` | Afficher ou non le nom. Le nom de l'entité_id sera choisi par défaut, à moins qu'il ne soit redéfini dans la propriété `name` ou dans n'importe quelle propriété `name` de l'état.|
| `show_state` | boolean | `false` | `true` \| `false` | Afficher l'état sur la carte. La valeur par défaut est false si elle n'est pas définie.
| `numeric_precision` | number | none | any number | n'importe quel nombre | Force la précision d'affichage de l'état à être avec les décimales de `numeric_precision`. |
| `show_icon` | boolean | `true` | `true` \| `false` | Afficher ou non l'icône. A moins d'être redéfinie dans `icon`, elle utilise l'icône par défaut de l'entité hass. |
| `show_units` | boolean | `true` | `true` \| `false` | Afficher ou cacher les unités d'un capteur, s'il y en a. |
| `show_label` | boolean | `false` | `true` \| `false` | Afficher ou masquer le `label` |
| `show_last_changed` | boolean | `false` | `true` \| `false` | Remplacer le label et afficher l'attribut `last_changed` d'une manière agréable (ex : `depuis 12 minutes`) |
| `show_entity_picture` | boolean | `false` | `true` \| `false` | Remplace l'icône par l'image de l'entité (s'il y en a une) ou l'image personnalisée (s'il y en a une). Il est possible de revenir à l'utilisation de l'icône si les deux ne sont pas définies. |
| `show_live_stream` | boolean | `false` | `true` \| `false` | Afficher le flux de la caméra (si l'entité est une caméra). Nécessite que le composant `stream:` soit activé dans la configuration de home-assistant. |
| `entity_picture` | string | optional | Peut être n'importe quel fichier `/local/*` ou une URL | Remplacera l'icône/l'image de l'entité par défaut par votre propre image. Le mieux est d'utiliser une image carrée. Vous pouvez également en définir une par état. Supporte les modèles, voir [templates](#javascript-templates) |
| `units` | string | optional | `Kb/s`, `lux`, ... | Les unités à afficher après l'état de l'entité sont surchargées ou définies. Si elles sont omises, les unités de l'entité sont utilisées. |
| `styles` | object list | optional |  | Voir [styles](#styles) |
| `extra_styles` | string | optional |  | Voir [styles](#styles) |
| `state` | object list | optional | Voir [State](#State) | Etat à utiliser pour la couleur, l'icône et le style du bouton. Plusieurs états peuvent être définis. |
| `confirmation` | object | optional | Voir [confirmation](#confirmation) | Display a confirmation popup |
| `lock` | object | optional | Voir [Lock Object](#lock-object) | Affiche un verrou sur le bouton. |
| `layout` | string | optional | See [Layout](#Layout) | La disposition du bouton peut être modifiée en utilisant cette option. |
| `custom_fields` | object | optional | See [Custom Fields](#Custom-Fields) |
| `variables` | object | optional | See [Variables](#Variables) |
| `card_size` | number | 3 | Any number | Configure la taille de la carte vue par la fonction de mise en page automatique de lovelace (lovelace multipliera la valeur par environ 50px) |
| `tooltip` | string | optional | Any string | (Non supporté sur les écrans tactiles) Vous pouvez configurer l'info-bulle affichée après avoir survolé la carte pendant 1,5 secondes. Supporte les modèles, voir [templates](#javascript-templates) |

### Action

Tous les champs sont compatibles avec les modèles, voir [templates](#javascript-templates).

| Name | Type | Default | Supported options | Description |
| --- | --- | --- | --- | --- |
| `action` | string | `toggle` | `more-info`, `toggle`, `call-service`, `none`, `navigate`, `url`, `assist` | Action à effectuer |
| `entity` | string | none | Any entity id | **Uniquement valable pour `action : more-info`** pour surcharger l'entité sur laquelle vous voulez appeler `more-info` |
| `target` | object | none |  | Ne fonctionne qu'avec `call-service`. Suit la [syntaxe de Home Assistant](https://www.home-assistant.io/docs/scripts/service-calls/#targeting-areas-and-devices) |
| `navigation_path` | string | none | Ex: `/lovelace/0/` | Chemin vers lequel naviguer (par exemple `/lovelace/0/`) quand l'action est définie comme `navigate` |
| `url_path` | string | none | Ex: `https://www.google.fr` | URL à ouvrir en cas de clic lorsque l'action est `url`. L'URL s'ouvrira dans un nouvel onglet. |
| `service` | string | none | Any service | Service à appeler (par exemple `media_player.media_play_pause`) lorsque `action` est définie comme `call-service`  |
| `data` or `service_data` | object | none | Any service data | Données de service à inclure (par exemple `entity_id : media_player.bedroom`) quand `action` est définie comme `call-service`. Si vos `data` nécessitent un `entity_id`, vous pouvez utiliser le keywork `entity`, qui appellera le service sur l'entité définie dans la configuration principale de cette carte. Utile pour les [configuration templates](#configuration-templates) |
| `haptic` | string | none | `success`, `warning`, `failure`, `light`, `medium`, `heavy`, `selection` | Retour d'information haptique pour l'application [Beta IOS](http://home-assistant.io/ios/beta) |
| `repeat` | number | none | eg: `500` | Pour une action de maintien, vous pouvez optionnellement configurer l'action pour qu'elle se répète tant que le bouton est maintenu enfoncé (par exemple, pour augmenter de manière répétée le volume d'un lecteur multimédia). Définissez ici le nombre de millisecondes entre les actions répétées. |
| `repeat_limit` | number | none | Ex: `5` | Pour l'action Hold et si `repeat` est défini, l'action cessera d'être appelée lorsque la `repeat_limit` aura été atteinte. |
| `confirmation` | object | none | See [confirmation](#confirmation) | Affiche une fenêtre popup de confirmation, remplace l'objet par défaut `confirmation`. |

### Confirmation

Une boîte de dialogue s'ouvre alors avant l'exécution de l'action.

| Name | Type | Default | Supported options | Description |
| --- | --- | --- | --- | --- |
| `text` | string | none | Any text | Ce texte sera affiché dans la fenêtre contextuelle. Supporte les modèles, voir [templates](#javascript-templates) |
| `exemptions` | array of users | none | `user: USER_ID` | Tout utilisateur déclaré dans cette liste ne verra pas la boîte de dialogue de confirmation. |

Exemple:

```yaml
confirmation:
  text: '[[[ return `Are you sure you want to toggle ${entity.attributes.friendly_name}?` ]]]'
  exemptions:
    - user: befc8496799848bda1824f2a8111e30a
```

### Lock Object

Un bouton normal s'affiche avec un symbole de verrouillage dans le coin. En cliquant sur le bouton, le verrou disparaît et le bouton peut être manœuvré pendant un délai de quelques secondes (5 par défaut).

| Name | Type | Default | Supported options | Description |
| --- | --- | --- | --- | --- |
| `enabled` | boolean | `false` | `true` \| `false` | Active ou désactive le verrou. Prend en charge les modèles, voir [templates](#javascript-templates) |
| `duration` | number | `5` | any number | Durée de l'état déverrouillé en secondes |
| `exemptions` | array of user id or username | none | `user: USER_ID` \| `username: test` | Tout utilisateur déclaré dans cette liste ne verra pas la boîte de dialogue de confirmation. Il peut s'agir d'un identifiant (`user`) ou d'un nom d'utilisateur (`username`). |
| `unlock` | string | `tap` | `tap` \| `hold` \| `double_tap` | Le type de clic qui déverrouillera le bouton |

Exemple:

```yaml
lock:
  enabled: '[[[ return entity.state === "on"; ]]]'
  duration: 10
  unlock: hold
  exemptions:
    - username: test
    - user: befc8496799848bda1824f2a8111e30a
```

Si vous souhaitez verrouiller le bouton pour tout le monde et désactiver la possibilité de déverrouillage, définissez l'objet "exemptions" comme suit `[]`:

```yaml
lock:
  enabled: true
  exemptions: []
```

Par défaut, le verrou est visible dans le coin supérieur droit. Si vous souhaitez déplacer la position du verrou, par exemple dans le coin inférieur droit, vous pouvez utiliser ce code :

```yaml
styles:
  lock:
    - justify-content: flex-end
    - align-items: flex-end
```

### State

| Name | Type | Default | Supported options | Description |
| --- | --- | --- | --- | --- |
| `operator` | string | `==` | See [Available Operators](#Available-operators) | L'opérateur utilisé pour comparer l'état actuel à la `value` |
| `value` | string/number | **required** (unless operator is `default`) | Si votre entité est un capteur avec des nombres, utilisez un nombre directement, sinon utilisez une string | La valeur qui sera comparée à l'état actuel de l'entité |
| `name` | string | optional | N'importe quelle chaîne de caractères, `'Alerte'`, `'Mon petit interrupteur est allumé'`, ...  | Si `show_name` est `true`, le nom à afficher pour cet état. S'il est défini, il utilise le nom de la configuration générale `name` et s'il n'est pas défini, il utilise le nom de l'entité. Supporte les modèles, voir [templates](#javascript-templates) |
| `icon` | string | optional | `mdi:battery` | L'icône à afficher pour cet état - La valeur par défaut est l'icône de l'entité. Cacher avec `show_icon : false`. Supporte les modèles, voir [templates](#javascript-templates) |
| `color` | string | `var(--primary-text-color)` | Any color, eg: `rgb(28, 128, 199)` or `blue` | La couleur de l'icône (si `color_type : icon`) ou de l'arrière-plan (si `color_type : card`) |
| `styles` | string | optional |  | Voir [styles](#styles) |
| `spin` | boolean | `false` | `true` \| `false` | L'icône doit-elle tourner pour cet état ? |
| `entity_picture` | string | optional | Peut être n'importe quel fichier `/local/*` ou une URL | Remplacera l'icône/l'image d'entité par défaut par votre propre image pour cet état. Le mieux est d'utiliser une image carrée. Supporte les modèles, voir [templates](#javascript-templates) |
| `label` | string | optional | Any string that you want | Affiche un label en dessous de la carte. Voir [Layouts](#layout) pour plus d'informations. Supporte les modèles, voir [templates](#javascript-templates) |
| `state_display` | string | optional | `On` | Si défini, surcharge la façon dont l'état est affiché. Supporte les modèles, voir [templates](#javascript-templates) |

### Available operators

L'ordre des éléments dans l'objet `state` est important. Le premier qui est `true` sera pris en compte. Le champ `value` de tous les opérateurs sauf `regex` supporte le templating, voir [templates](#javascript-templates)

| Operator | `value` example | Description |
| :-: | --- | --- |
| `<` | `5` | L'état actuel est inférieur à `value` |
| `<=` | `4` | L'état actuel est inférieur ou égal à `value` |
| `==` | `42` or `'on'` | **C'est la valeur par défaut si aucun opérateur n'est spécifié.** L'état actuel est égal (`==` javascript) à`value` |
| `>=` | `32` | L'état actuel est supérieur ou égal à `value` |
| `>` | `12` | L'état actuel est supérieur à `value` |
| `!=` | `'normal'` | L'état actuel n'est pas égal (`!=` javascript) à `value` |
| `regex` | `'^norm.*$'` | `value` l'expression regex appliquée à l'état actuel correspond à |
| `template` |  | Voir [templates](#javascript-templates) par exemple. `value` doit être une expression javascript qui retourne un booléen. Si le booléen est vrai, il correspondra à cet état |
| `default` | N/A | Si rien ne correspond, c'est cette valeur qui est utilisée |

### Layout

Cette option vous permet de modifier la présentation de la carte.

Elle est entièrement compatible avec toutes les options de `show_*`. Assurez-vous de mettre `show_state : true` si vous voulez afficher l'état de la carte.

Plusieurs valeurs sont possibles, voir l'image ci-dessous pour des exemples :

- `vertical` (valeur par défaut si rien n'est fourni) : Tout est centré verticalement les uns sur les autres.
- `icon_name_state` : Tout est aligné horizontalement, le nom et l'état sont concaténés, l'étiquette est centrée en dessous.
- `name_state` : L'icône se trouve au-dessus du nom et de l'état concaténés sur une ligne, l'étiquette se trouve en dessous.
- `icon_name` : L'icône et le nom sont alignés horizontalement, l'état et l'étiquette sont centrés en dessous.
- `icon_state` : L'icône et l'état sont alignés horizontalement, le nom et l'étiquette sont centrés en dessous.
- `icon_label` : L'icône et l'étiquette sont alignées horizontalement, le nom et l'état sont centrés en dessous.
- `icon_name_state2nd` : L'icône, le nom et l'état sont alignés horizontalement, le nom est au-dessus de l'état, l'étiquette est en dessous du nom et de l'état.
- `icon_state_name2nd` : L'icône, le nom et l'état sont alignés horizontalement, l'état est au-dessus du nom, l'étiquette est en dessous du nom et de l'état.

![layout_image](examples/layout.png)

### `triggers_update`

Ce champ définit les entités qui doivent déclencher une mise à jour de la carte elle-même (cette règle ne s'applique pas aux cartes imbriquées dans les champs personnalisés, car elles sont toujours mises à jour avec le dernier état. C'est prévu et rapide !) Ceci a été introduit dans la version 3.3.0 pour réduire la charge sur le frontend.

Si vous n'avez pas de modèles javascript `[[[ ]]]` dans votre configuration, vous n'avez rien à faire, sinon lisez la suite.

Par défaut, la carte se met à jour lorsque l'entité principale de la configuration est mise à jour. Dans tous les cas, la carte va analyser votre code et chercher des entités qu'elle peut faire correspondre (**elle ne fait que correspondre à `states['ENTITY_ID']`**) :

```js
return states['switch.myswitch'].state // will match switch.myswitch
 // but
 const test = switch.myswitch
 return states[test].state // will not match anything
```

Dans ce deuxième cas, deux possibilités s'offrent à vous :

- Fixer la valeur de `triggers_update` à `all` (C'était le comportement de button-card < 3.3.0)

  ```yaml
  triggers_update: all
  ```

- Fixe la valeur de `triggers_update` à une liste d'entités. Lorsque l'une des entités de cette liste est mise à jour, la carte est mise à jour. La logique est la même que celle de l'intégration interne des `* templates` de l'aide à domicile (voir [ici](https://www.home-assistant.io/integrations/binary_sensor.template/#entity_id) par exemple):

  ```yaml
  type: custom:button-card
  entity: sensor.mysensor # No need to repeat this one in the triggers_update, it is added by default
  triggers_update:
    - switch.myswitch
    - light.mylight
  ```

Si votre entité, n'importe quelle entité dans le champ `triggers_update` ou n'importe quelle entité correspondant à vos templates sont un groupe et que vous voulez mettre à jour la carte si l'une des entités imbriquées dans ce groupe met à jour son état, alors vous pouvez mettre `group_expand` à `true`. Cela fera le travail pour vous et vous n'aurez pas à spécifier manuellement la liste complète des entités dans `triggers_update`.

### Javascript Templates

Le rendu du modèle utilise un format spécial. Tous les champs où les modèles sont pris en charge supportent également le texte brut. Pour activer la fonction de template pour un tel champ, vous devez placer la fonction javascript entre 3 crochets : `[[[ fonction javascript ici ]]]`

N'oubliez pas de mettre la fonction entre guillemets si elle tient sur une seule ligne :

```yaml
name: '[[[ if (entity.state > 42) return "Above 42"; else return "Below 42" ]]]'
name: >
  [[[
    if (entity.state > 42)
      return "Above 42";
    else
      return "Below 42";
  ]]]
```
Voici les champs de configuration qui supportent la création de modèles :


- `name` (supporte aussi le rendu HTML) : Ce champ doit retourner une chaîne de caractères ou un objet `` html`<elt></elt>`.
- `state_display` (Supporte aussi le rendu HTML) : Ceci doit retourner une chaîne ou un objet `` html`<elt></elt>`.
- `label` (Supporte aussi le rendu HTML) : Ceci doit retourner une chaîne ou un objet `` html`<elt></elt>` `.
- `entity_picture` : Ceci doit retourner un chemin vers un fichier ou une url sous forme de chaîne de caractères.
- `icon` : Cet objet doit retourner une chaîne de caractères au format `mdi:icon`
- Tous les styles de l'objet style : Ceci doit retourner une chaîne de caractères
- Toutes les valeurs de l'objet state, sauf lorsque l'opérateur est `regex`
  - `operator : template` : La fonction pour `value` doit retourner un booléen.
  - Else : La fonction pour `value` doit retourner une chaîne de caractères ou un nombre.
- Tous les `custom_fields` (supportent aussi le rendu HTML) : Cette fonction doit retourner une chaîne de caractères ou un objet `` html`<elt></elt>`.
- Tous les `styles` : Chaque entrée doit retourner une chaîne (Voir [ici](#custom-fields) pour quelques exemples)
- Le champ `extra_styles`.
- Tous les champs de `*_action`
- Le texte de confirmation (`confirmation.text`)
- Le verrou activé ou non (`lock.enabled`)
- tous les paramètres `card` dans un `custom_field`
- toutes les `variables`

Champs spéciaux qui supportent les modèles mais qui ne sont **évalués qu'une seule fois**, lorsque la configuration est chargée. Les erreurs dans ces modèles ne seront visibles que dans la console javascript et la carte ne s'affichera pas dans ce cas :

- `entity` : Vous pouvez utiliser des modèles JS pour l'entité (`entity`) de la configuration de la carte. C'est principalement utile lorsque vous définissez votre entité comme une entrée dans `variables`. Celle-ci est évaluée une seule fois lors du chargement de la configuration.

  ```yaml
  type: custom:button-card
  variables:
    my_entity: switch.skylight
  entity: '[[[ return variables.my_entity; ]]]'
  ```

- `triggers_update` : Utile lorsque vous définissez plusieurs entités dans `variables` pour les utiliser dans la carte. Par exemple :

  ```yaml
  type: custom:button-card
  variables:
    my_entity: switch.skylight
    my_other_entity: light.bedroom
  entity: '[[[ return variables.my_entity; ]]]'
  label: '[[[ return localize(variables.my_other_entity) ]]]'
  show_label: true
  triggers_update:
    - '[[[ return variables.my_entity; ]]]'
    - '[[[ return variables.my_other_entity; ]]]'
  ```

Dans le code javascript, vous aurez accès à ces variables :


- `this` : L'élément "carte-bouton" lui-même (c'est un truc avancé, n'y touchez pas)
- `entity` : L'objet de l'entité courante, si l'entité est définie dans la carte
- `states` : Un objet avec tous les états de toutes les entités (équivalent à `hass.states`)
- `user` : L'objet utilisateur (équivalent à `hass.user`)
- `hass` : L'objet `hass` complet
- `variables` : un objet contenant toutes les variables définies dans la configuration. Voir [Variables](#variables)
- Fonctions d'aide disponibles à travers l'objet `helpers` :
  - `helpers.localize(entity, state ?, numeric_precision ?, show_units ?, units ?)` : une fonction qui localise un état dans votre langue (par exemple `helpers.localize(entity)`) et retourne une chaîne de caractères. Prend un objet entité comme argument (pas l'état de l'entité car nous avons besoin du contexte) et prend des arguments optionnels. Fonctionne également avec les états numériques.
    - Si `state` n'est pas fourni, il localise l'état de l'entité (ex : `helpers.localize(entity)` ou `helpers.localize(states['weather.your_city'])`).
    - Si `state` est fourni, il localise `state` dans le contexte de l'entité (ex : `helpers.localize(entity, entity.attributes.forecast[0].condition)` ou `helpers.localize(states['weather.your_city'], states['weather.your_city'].attributes.forecast[0].condition)`)
    - `numeric_precision` (nombre ou `'card'`. La valeur par défaut est `undefined`) : Pour les états qui sont des nombres, forcer la précision au lieu de laisser HA décider pour vous. Si la valeur est `'card'`, il utilisera la `precision_numérique` de la configuration principale. Si `undefined`, il utilisera la valeur par défaut de l'entité que vous souhaitez afficher. Cette dernière est la valeur par défaut.
    - `show_units` (booléen. La valeur par défaut est `true`) : Affiche ou non les unités. La valeur par défaut est de les afficher.
    - `units` (chaîne de caractères) : Force les unités à être la valeur de ce paramètre.
    - Pour sauter un ou plusieurs paramètres lors de l'appel de la fonction, utilisez `undefined`. Par exemple, `helpers.localize(states['sensor.temperature'], undefined, 1, undefined, 'Celcius')`
  - Aides au formatage de la date, de l'heure et de la date et de l'heure, toutes localisées (prend une chaîne de caractères ou un objet `Date` en entrée) :
    - `helpers.formatTime24h(time)` : 21:15
    - `helpers.formatDateWeekdayDay(date)` : Mardi 10 août
    - `helpers.formatDate(date)` : 10 août 2021
    - `helpers.formatDateNumeric(date)` : 10/08/2021
    - `helpers.formatDateShort(date)` : Aug 10
    - `helpers.formatDateMonthYear(date)` : Août 2021
    - `helpers.formatDateMonth(date)` : Août
    - `helpers.formatDateYear(date)` : 2021
    - `helpers.formatDateWeekday(date)` : Lundi
    - `helpers.formatDateWeekdayShort(date)` : Mon
    - `helpers.formatDateTime(datetime)` : 9 août 2021, 8:23 AM
    - `helpers.formatDateTimeNumeric(datetime)` : Aug 9, 2021, 8:23 AM
    - `helpers.formatDateTimeWithSeconds(datetime)` : Aug 9, 8:23 AM
    - `helpers.formatShortDateTime(datetime)` : 9 août 2021, 8:23:15 AM
    - `helpers.formatShortDateTimeWithYear(datetime)` : 9/8/2021, 8:23 AM
    - Exemple : `return helpers.formatDateTime(entity.attribute.last_changed)`
  - `helpers.relativeTime(date, capitalize ? = false)` : Retourne un modèle lit-html qui rendra une heure relative et la mettra à jour automatiquement. `date` doit être une chaîne de caractères. `capitalize` est un booléen optionnel, s'il vaut `true`, la première lettre sera en majuscule. Utilisation par exemple : `return helpers.relativeTime(entity.last_changed)`

Voir [ici](#templates-support) pour quelques exemples ou [ici](#custom-fields) pour des trucs avancés utilisant les templates !

### Styles

Toutes les entrées de style prennent en charge le Templating, voir [ici](#custom-fields) pour quelques exemples.

#### Easy styling options

Pour chaque élément de la carte, les styles peuvent être définis à deux endroits :

- dans la partie principale de la configuration
- dans chaque état

Les styles définis dans chaque état sont **additifs** à ceux définis dans la partie principale de la configuration. Dans la partie `state`, définissez simplement les styles spécifiques à votre état actuel et gardez les styles communs dans la partie principale de la configuration.

Les membres de l'objet `style` sont :

- `card` : styles pour la carte elle-même. Les styles définis ici seront appliqués à l'ensemble de la carte et à son contenu, à moins qu'ils ne soient redéfinis dans les éléments ci-dessous.
- `icon` : styles pour l'icône
- `entity_picture` : styles pour l'image (si elle existe)
- `name` : styles pour le nom
- `state` : styles pour l'état
- `label` : styles pour le label
- `lock` : styles pour l'icône du verrou (voir [ici](https://github.com/custom-cards/button-card/blob/master/src/styles.ts#L73-L86) pour le style par défaut)
- `tooltip`: styles pour la superposition de l'info-bulle (voir [ici](https://github.com/custom-cards/button-card/blob/master/src/styles.ts#L30-L46))
- `custom_fields`: styles pour chacun de vos champs personnalisés. Voir [Custom Fields](#custom-fields)

```yaml
- type: custom:button-card
  [...]
  styles:
    card:
      - xxxx: value
    icon:
      - xxxx: value
    entity_picture:
      - xxxx: value
    name:
      - xxxx: value
    state:
      - xxxx: value
    label:
      - xxxx: value
  state:
    - value: 'on'
      styles:
        card:
          - yyyy: value
        icon:
          - yyyy: value
        entity_picture:
          - yyyy: value
        name:
          - yyyy: value
        state:
          - yyyy: value
        label:
          - yyyy: value
```

This will render:

- The `card` with the styles `xxxx: value` **and** `yyyy: value` applied
- Same for all the others.

See [styling](#styling) for a complete example.

#### Light entity color variable

If a light entity is assigned to the button, then:

- the CSS variable `--button-card-light-color` will contain the current light color
- the CSS variable `--button-card-light-color-no-temperature` will contain the current light without the temperature

You can use them both in other parts of the button. When off, it will be set to `var(--paper-item-icon-color)`

![css-var](examples/color-variable.gif)

```yaml
styles:
  name:
    - color: var(--button-card-light-color)
  card:
    - -webkit-box-shadow: 0px 0px 9px 3px var(--button-card-light-color)
    - box-shadow: 0px 0px 9px 3px var(--button-card-light-color)
```

#### ADVANCED styling options

For advanced styling, there are 2 other options in the `styles` config object:

- `grid`: mainly layout for the grid
- `img_cell`: mainly how you position your icon in its cell

This is how the button is constructed (HTML elements):

![elements in the button](examples/button-card-elements.png)

The `grid` element uses CSS grids to design the layout of the card:

- `img_cell` element is going to the `grid-area: i` by default
- `name` element is going to the `grid-area: n` by default
- `state` element is going to the `grid-area: s` by default
- `label` element is going to the `grid-area: l` by default

You can see how the default layouts are constructed [here](./src/styles.ts#L152) and inspire yourself with it. We'll not support advanced layout questions here, please use [Home Assistant's community forum][forum] for that.

To learn more, please use Google and this [excellent guide about CSS Grids](https://css-tricks.com/snippets/css/complete-guide-grid/) :)

For a quick overview on the grid-template-areas attribute, the following example should get you started:

```yaml
- grid-template-areas: '"i n s" "i n s" "i n l"'
```

If we take the value and orient it into rows and columns, you begin to see the end result.

```
"i n s"
"i n s"
"i n l"
```

The end product will results in the following grid layout

![button card grid layout example with callouts](examples/button-card-grid-layout-example-with-callouts.png)

Some examples:

- label on top:

  ```yaml
  styles:
    grid:
      - grid-template-areas: '"l" "i" "n" "s"'
      - grid-template-rows: min-content 1fr min-content min-content
      - grid-template-columns: 1fr
  ```

- icon on the right side (by overloading an existing layout):

  ```yaml
  - type: 'custom:button-card'
    entity: sensor.sensor1
    layout: icon_state_name2nd
    show_state: true
    show_name: true
    show_label: true
    label: label
    styles:
      grid:
        - grid-template-areas: '"n i" "s i" "l i"'
        - grid-template-columns: 1fr 40%
  ```

- Apple Homekit-like buttons:

  ![apple-like-buttons](examples/apple_style.gif)

  ```yaml
  - type: custom:button-card
    entity: switch.skylight
    name: <3 Apple
    icon: mdi:fire
    show_state: true
    styles:
      card:
        - width: 100px
        - height: 100px
      grid:
        - grid-template-areas: '"i" "n" "s"'
        - grid-template-columns: 1fr
        - grid-template-rows: 1fr min-content min-content
      img_cell:
        - align-self: start
        - text-align: start
      name:
        - justify-self: start
        - padding-left: 10px
        - font-weight: bold
        - text-transform: lowercase
      state:
        - justify-self: start
        - padding-left: 10px
    state:
      - value: 'off'
        styles:
          card:
            - filter: opacity(50%)
          icon:
            - filter: grayscale(100%)
  ```

#### Injecting CSS with `extra_styles`

**Note**: `extra_styles` **MUST NOT** be used on the first button-card of the current view, else it will be applied to all the cards in all Lovelace. **It is not possible to fix this behaviour.**

You can inject any CSS style you want using this config option. It is useful if you want to inject CSS animations for example. This field supports [templates](#javascript-templates).

An example is better than words:

![change_background](examples/loop_background.gif)

```yaml
- type: custom:button-card
  name: Change Background
  aspect_ratio: 2/1
  extra_styles: |
    @keyframes bgswap1 {
      0% {
        background-image: url("/local/background1.jpg");
      }
      25% {
        background-image: url("/local/background1.jpg");
      }
      50% {
        background-image: url("/local/background2.jpg");
      }
      75% {
        background-image: url("/local/background2.jpg");
      }
      100% {
        background-image: url("/local/background1.jpg");
      }
    }
  styles:
    card:
      - animation: bgswap1 10s linear infinite
      - background-size: cover
    name:
      - color: white
```

### Custom Fields

Custom fields support, using the `custom_fields` object, enables you to create your own fields on top of the pre-defined ones (name, state, label and icon). This is an advanced feature which leverages (if you require it) the CSS Grid.

Custom fields also support embeded cards, see [example below](#custom_fields_card_example).

Each custom field supports its own styling config, the name needs to match between both objects needs to match:

```yaml
- type: custom:button-card
  [...]
  custom_fields:
    test_element: My test element
  styles:
    custom_fields:
      test_element:
        - color: red
        - font-size: 13px
```

Examples are better than a long text, so here you go:

- Placing an element wherever you want (that means bypassing the grid). Set the grid to `position: relative` and set the element to `position: absolute`

  ![custom_fields_1](examples/custom_fields_1.gif)

  ```yaml
  - type: custom:button-card
    icon: mdi:lightbulb
    aspect_ratio: 1/1
    name: Nb lights on
    styles:
      grid:
        - position: relative
      custom_fields:
        notification:
          - background-color: >
              [[[
                if (states['input_number.test'].state == 0)
                  return "green";
                return "red";
              ]]]


          - border-radius: 50%
          - position: absolute
          - left: 60%
          - top: 10%
          - height: 20px
          - width: 20px
          - font-size: 8px
          - line-height: 20px
    custom_fields:
      notification: >
        [[[ return Math.floor(states['input_number.test'].state / 10) ]]]
  ```

- Or you can use the grid. Each element will have it's name positioned as the `grid-area`:

  ![custom_fields_2](examples/custom_fields_2.png)

  ```yaml
  - type: custom:button-card
    entity: 'sensor.raspi_temp'
    icon: 'mdi:raspberry-pi'
    aspect_ratio: 1/1
    name: HassOS
    styles:
      card:
        - background-color: '#000044'
        - border-radius: 10%
        - padding: 10%
        - color: ivory
        - font-size: 10px
        - text-shadow: 0px 0px 5px black
        - text-transform: capitalize
      grid:
        - grid-template-areas: '"i temp" "n n" "cpu cpu" "ram ram" "sd sd"'
        - grid-template-columns: 1fr 1fr
        - grid-template-rows: 1fr min-content min-content min-content min-content
      name:
        - font-weight: bold
        - font-size: 13px
        - color: white
        - align-self: middle
        - justify-self: start
        - padding-bottom: 4px
      img_cell:
        - justify-content: start
        - align-items: start
        - margin: none
      icon:
        - color: >
            [[[
              if (entity.state < 60) return 'lime';
              if (entity.state >= 60 && entity.state < 80) return 'orange';
              else return 'red';
            ]]]


        - width: 70%
        - margin-top: -10%
      custom_fields:
        temp:
          - align-self: start
          - justify-self: end
        cpu:
          - padding-bottom: 2px
          - align-self: middle
          - justify-self: start
          - --text-color-sensor: '[[[ if (states["sensor.raspi_cpu"].state > 80) return "red"; ]]]'
        ram:
          - padding-bottom: 2px
          - align-self: middle
          - justify-self: start
          - --text-color-sensor: '[[[ if (states["sensor.raspi_ram"].state > 80) return "red"; ]]]'
        sd:
          - align-self: middle
          - justify-self: start
          - --text-color-sensor: '[[[ if (states["sensor.raspi_sd"].state > 80) return "red"; ]]]'
    custom_fields:
      temp: >
        [[[
          return `<ha-icon
            icon="mdi:thermometer"
            style="width: 12px; height: 12px; color: yellow;">
            </ha-icon><span>${entity.state}°C</span>`
        ]]]


      cpu: >
        [[[
          return `<ha-icon
            icon="mdi:server"
            style="width: 12px; height: 12px; color: deepskyblue;">
            </ha-icon><span>CPU: <span style="color: var(--text-color-sensor);">${states['sensor.raspi_cpu'].state}%</span></span>`
        ]]]


      ram: >
        [[[
          return `<ha-icon
            icon="mdi:memory"
            style="width: 12px; height: 12px; color: deepskyblue;">
            </ha-icon><span>RAM: <span style="color: var(--text-color-sensor);">${states['sensor.raspi_ram'].state}%</span></span>`
        ]]]


      sd: >
        [[[
          return `<ha-icon
            icon="mdi:harddisk"
            style="width: 12px; height: 12px; color: deepskyblue;">
            </ha-icon><span>SD: <span style="color: var(--text-color-sensor);">${states['sensor.raspi_sd'].state}%</span></span>`
        ]]]
  ```

- <a name="custom_fields_card_example"></a>Or you can embed a card (or multiple) inside the button card (note, this configuration uses [card-mod](https://github.com/thomasloven/lovelace-card-mod) to remove the `box-shadow` of the sensor card.

- This is what the `style` inside the embedded card is for):

  ![custom_fields_3](examples/custom_fields_card.png)

  ```yaml
  - type: custom:button-card
    aspect_ratio: 1/1
    custom_fields:
      graph:
        card:
          type: sensor
          entity: sensor.sensor1
          graph: line
          style: |
            ha-card {
              box-shadow: none;
            }
    styles:
      custom_fields:
        graph:
          - filter: opacity(50%)
          - overflow: unset
      card:
        - overflow: unset
      grid:
        - grid-template-areas: '"i" "n" "graph"'
        - grid-template-columns: 1fr
        - grid-template-rows: 1fr min-content min-content

    entity: light.test_light
    hold_action:
      action: more-info
  ```

To skip evaluating the templates in a custom_field (eg. you embed a `custom:button-card` inside a Custom Field), then you have to set `do_not_eval` to `true`.

```yaml
type: custom:button-card
styles:
  grid:
    - grid-template-areas: "'test1' 'test2'"
variables:
  b: 42
custom_fields:
  test1:
    card:
      type: custom:button-card
      variables:
        c: 42
      ## This will return: B: 42 / C: undefined
      ## as it is evaluated in the context of the
      ## main card (which doesn't know about c)
      name: '[[[ return `B: ${variables.b} / C: ${variables.c}` ]]]'
  test2:
    ## This stops the evaluation of js templates
    ## for the card object in this custom field
    do_not_eval: true
    card:
      type: custom:button-card
      variables:
        c: 42
      ## This will return: B: undefined / C: 42
      ## as it is evaluated in the context of the local button-card
      ## inside the custom_field (which doesn't know about b)
      name: '[[[ return `B: ${variables.b} / C: ${variables.c}` ]]]'
```

### Configuration Templates

#### General

- Define your config template in the main lovelace configuration and then use it in your button-card. This will avoid a lot of repetitions! It's basically YAML anchors, but without using YAML anchors and is very useful if you split your config in multiple files 😄
- You can overload any parameter with a new one
- You can merge states together **by `id`** when using templates. The states you want to merge have to have the same `id`. This `id` parameter is new and can be anything (string, number, ...). States without `id` will be appended to the state array. Styles embedded in a state are merged together as usual. See [here](#merging-state-by-id) for an example.
- You can also inherit another template from within a template.
- You can inherit multiple templates at once by making it an array. In this case, the templates will be merged together with the current configuration in the order they are defined. This happens recursively.

  ```yaml
  type: custom:button-card
  template:
    - template1
    - template2
  ```

  The button templates will be applied in the order they are defined: `template2` will be merged with `template1` and then the local config will be merged with the result. You can still chain templates together (ie. define template in a button-card template. It will follow the path recursively).

Make sure which type of lovelace dashboard you are using before changing the main lovelace configuration:

- **`managed`** changes are managed by lovelace ui - add the template configuration to configuration in raw editor
  - go to your dashboard
  - click three dots and `Edit dashboard` button
  - click three dots again and click `Raw configuration editor` button
- **`yaml`** - add template configuration to your `ui-lovelace.yaml`

```yaml
button_card_templates:
  header:
    styles:
      card:
        - padding: 5px 15px
        - background-color: var(--paper-item-icon-active-color)
      name:
        - text-transform: uppercase
        - color: var(--primary-background-color)
        - justify-self: start
        - font-weight: bold
      label:
        - text-transform: uppercase
        - color: var(--primary-background-color)
        - justify-self: start
        - font-weight: bold

  header_red:
    template: header
    styles:
      card:
        - background-color: '#FF0000'

  my_little_template: [...]
```

And then where you use button-card, you can apply this template, and/or overload it:

```yaml
- type: custom:button-card
  template: header_red
  name: My Test Header
```

#### Merging state by id

Example to merge state by `id`:

```yaml
button_card_templates:
  sensor:
    styles:
      card:
        - font-size: 16px
        - width: 75px
    tap_action:
      action: more-info
    state:
      - color: orange
        value: 75
        id: my_id

  sensor_humidity:
    template: sensor
    icon: 'mdi:weather-rainy'
    state:
      - color: 'rgb(255,0,0)'
        operator: '>'
        value: 50
      - color: 'rgb(0,0,255)'
        operator: '<'
        value: 25

  sensor_test:
    template: sensor_humidity
    state:
      - color: pink
        id: my_id
        operator: '>'
        value: 75
        styles:
          name:
            - color: '#ff0000'
############### Used like this ##############
  - type: custom:button-card
    template: sensor_test
    entity: input_number.test
    show_entity_picture: true
```

Will result in this state object for your button (styles, operator and color are overridden for the `id: my_id` as you can see):

```yaml
state:
  - color: pink
    operator: '>'
    value: 75
    styles:
      name:
        - color: '#ff0000'
  - color: 'rgb(255,0,0)'
    operator: '>'
    value: 50
  - color: 'rgb(0,0,255)'
    operator: '<'
    value: 25
```

#### Variables

You can add variables to your templates and overload them in the instance of your button card. This lets you easily work with templates without the need to redefine everything for a small change.

An example below:

```yaml
button_card_templates:
  variable_test:
    variables:
      var_name: "var_value"
      var_name2: "var_value2"
    name: '[[[ return variables.var_name ]]]'

[...]

- type: custom:button-card
  template: variable_test
  entity: sensor.test
  # name will be "var_value"

- type: custom:button-card
  template: variable_test
  entity: sensor.test
  variables:
    var_name: "My local Value"
 # name will be "My local Value"
```

Variables are evaluated in their alphabetical order based on their name. That means a variable named `b` can depend on a variable named `a`, but variable named `a` can't depend on a variable named `b`.

```yaml
### This works
variables:
  index: 2
  value: '[[[ return variables.index + 2; ]]]'
name: '[[[ return variable.value; ]]]' # would return 4

### This doesn't work
variables:
  z_index: 2
  value: '[[[ return variables.z_index + 2; ]]]' # This would fail because z comes after v in the alphabet.
name: '[[[ return variable.value; ]]]'
```

## Installation

### Manual Installation

1. Download the [button-card](http://www.github.com/custom-cards/button-card/releases/latest/download/button-card.js)
2. Place the file in your `config/www` folder
3. Include the card code in your `ui-lovelace-card.yaml`

   ```yaml
   title: Home
   resources:
     - url: /local/button-card.js
       type: module
   ```

4. Write configuration for the card in your `ui-lovelace.yaml`

### Installation and tracking with `hacs`

1. Make sure the [HACS](https://github.com/custom-components/hacs) component is installed and working.
2. Search for `button-card` and add it through HACS
3. Add the configuration to your `ui-lovelace.yaml`

   ```yaml
   resources:
     - url: /hacsfiles/button-card/button-card.js
       type: module
   ```

4. Refresh home-assistant.

## Examples

Show a button for the air conditioner (blue when on, `var(--disabled-text-color)` when off):

![ac](examples/ac.png)

```yaml
- type: 'custom:button-card'
  entity: switch.ac
  icon: mdi:air-conditioner
  color: rgb(28, 128, 199)
```

Redefine the color when the state if off to red:

```yaml
- type: 'custom:button-card'
  entity: switch.ac
  icon: mdi:air-conditioner
  color: rgb(28, 128, 199)
  state:
    - value: 'off'
      color: rgb(255, 0, 0)
```

---

Show an ON/OFF button for the home_lights group:

![no-icon](examples/no_icon.png)

```yaml
- type: 'custom:button-card'
  entity: group.home_lights
  show_icon: false
  show_state: true
```

---

Light entity with custom icon and "more info" pop-in:

![sofa](examples/sofa.png)

```yaml
- type: 'custom:button-card'
  entity: light.living_room_lights
  icon: mdi:sofa
  color: auto
  tap_action:
    action: more-info
```

---

Light card with card color type, name, and automatic color:

![color](examples/color.gif)

```yaml
- type: 'custom:button-card'
  entity: light._
  icon: mdi:home
  color: auto
  color_type: card
  tap_action:
    action: more-info
  name: Home
  styles:
    card:
      - font-size: 12px
      - font-weight: bold
```

---

Horizontal stack with :

- 2x blank cards
- 1x volume up button with service call
- 1x volume down button with service call
- 2x blank cards

![volume](examples/volume.png)

```yaml
- type: horizontal-stack
  cards:
    - type: 'custom:button-card'
      color_type: blank-card
    - type: 'custom:button-card'
      color_type: blank-card
    - type: 'custom:button-card'
      color_type: card
      color: rgb(223, 255, 97)
      icon: mdi:volume-plus
      tap_action:
        action: call-service
        service: media_player.volume_up
        data:
          entity_id: media_player.living_room_speaker
    - type: 'custom:button-card'
      color_type: card
      color: rgb(223, 255, 97)
      icon: mdi:volume-minus
      tap_action:
        action: call-service
        service: media_player.volume_down
        data:
          entity_id: media_player.living_room_speaker
    - type: 'custom:button-card'
      color_type: blank-card
    - type: 'custom:button-card'
      color_type: blank-card
```

---

Vertical Stack with :

- 1x label card
- Horizontal Stack with :
  - 1x Scene 1 Button
  - 1x Scene 2 Button
  - 1x Scene 3 Button
  - 1x Scene 4 Button
  - 1x Scene Off Button

![scenes](examples/scenes.png)

```yaml
- type: vertical-stack
  cards:
    - type: 'custom:button-card'
      color_type: label-card
      color: rgb(44, 109, 214)
      name: Kitchen
    - type: horizontal-stack
      cards:
        - type: 'custom:button-card'
          entity: switch.kitchen_scene_1
          color_type: card
          color: rgb(66, 134, 244)
          icon: mdi:numeric-1-box-outline
        - type: 'custom:button-card'
          entity: switch.kitchen_scene_2
          color_type: card
          color: rgb(66, 134, 244)
          icon: mdi:numeric-2-box-outline
        - type: 'custom:button-card'
          entity: switch.kitchen_scene_3
          color_type: card
          color: rgb(66, 134, 244)
          icon: mdi:numeric-3-box-outline
        - type: 'custom:button-card'
          entity: switch.kitchen_scene_4
          color_type: card
          color: rgb(66, 134, 244)
          icon: mdi:numeric-4-box-outline
        - type: 'custom:button-card'
          entity: switch.kitchen_off
          color_type: card
          color: rgb(66, 134, 244)
          icon: mdi:eye-off-outline
```

### Configuration with states

Input select card with select next service and custom color and icon for states. In the example below the icon `mdi:cube-outline` will be used when value is `sleeping` and `mdi:cube` in other cases.

![cube](examples/cube.png)

#### Default behavior

If you don't specify any operator, `==` will be used to match the current state against the `value`

```yaml
- type: 'custom:button-card'
  entity: input_select.cube_mode
  icon: mdi:cube
  tap_action:
    action: call-service
    service: input_select.select_next
    data:
      entity_id: input_select.cube_mode
  show_state: true
  state:
    - value: 'sleeping'
      color: var(--disabled-text-color)
      icon: mdi:cube-outline
    - value: 'media'
      color: rgb(5, 147, 255)
    - value: 'light'
      color: rgb(189, 255, 5)
```

#### With Operator on state

The definition order matters, the first item to match will be the one selected.

```yaml
- type: "custom:button-card"
  entity: sensor.temperature
  show_state: true
  state:
    - value: 15
      operator: '<='
      color: blue
      icon: mdi:thermometer-minus
    - value: 25
      operator: '>='
      color: red
      icon: mdi:thermometer-plus
    - operator: 'default' # used if nothing matches
      color: yellow
      icon: mdi: thermometer
      styles:
        card:
          - opacity: 0.5
```

#### `tap_action` Navigate

Buttons can link to different views using the `navigate` action:

```yaml
- type: 'custom:button-card'
  color_type: label-card
  icon: mdi:home
  name: Go To Home
  tap_action:
    action: navigate
    navigation_path: /lovelace/0
```

The `navigation_path` also accepts any Home Assistant internal URL such as /dev-info or /hassio/dashboard for example.

#### blink

You can make the whole button blink:

![blink-animation](examples/blink-animation.gif)

```yaml
- type: 'custom:button-card'
  color_type: card
  entity: binary_sensor.intruder
  name: Intruder Alert
  state:
    - value: 'on'
      color: red
      icon: mdi:alert
      styles:
        card:
          - animation: blink 2s ease infinite
    - operator: default
      color: green
      icon: mdi:shield-check
```

### Play with width, height and icon size

Through the `styles` you can specify the `width` and `height` of the card, and also the icon size through the main `size` option. Playing with icon size will growth the card unless a `height` is specified.

If you specify a width for the card, it has to be in `px`. All the cards without a `width` defined will use the remaining space on the line.

![height-width](examples/width_height.png)

```yaml
- type: horizontal-stack
  cards:
    - type: 'custom:button-card'
      entity: light.test_light
      color: auto
      name: s:default h:200px
      styles:
        card:
          - height: 200px
    - type: 'custom:button-card'
      entity: light.test_light
      color_type: card
      color: auto
      name: s:100% h:200px
      size: 100%
      styles:
        card:
          - height: 200px
    - type: 'custom:button-card'
      entity: light.test_light
      color_type: card
      color: auto
      size: 10%
      name: s:10% h:200px
      styles:
        card:
          - height: 200px
- type: horizontal-stack
  cards:
    - type: 'custom:button-card'
      entity: light.test_light
      color: auto
      name: 60px
      styles:
        card:
          - height: 60px
          - width: 60px
    - type: 'custom:button-card'
      entity: light.test_light
      color_type: card
      color: auto
      name: 80px
      styles:
        card:
          - height: 80px
          - width: 30px
    - type: 'custom:button-card'
      entity: light.test_light
      color_type: card
      color: auto
      name: 300px
      styles:
        card:
          - height: 300px
```

### Templates Support

#### Playing with label templates

![label_template](examples/labels.png)

```yaml
- type: 'custom:button-card'
  color_type: icon
  entity: light.test_light
  label: >
    [[[
      var bri = states['light.test_light'].attributes.brightness;
      return 'Brightness: ' + (bri ? bri : '0') + '%';
    ]]]


  show_label: true
  size: 15%
  styles:
    card:
      - height: 100px
- type: 'custom:button-card'
  color_type: icon
  entity: light.test_light
  layout: icon_label
  label: >
    [[[
      return 'Other State: ' + states['switch.skylight'].state;
    ]]]


  show_label: true
  show_name: false
  styles:
    card:
      - height: 100px
```

#### State Templates

The javascript code inside `value` needs to return `true` of `false`.

Example with `template`:

```yaml
- type: 'custom:button-card'
  color_type: icon
  entity: switch.skylight
  show_state: true
  show_label: true
  state:
    - operator: template
      value: >
        [[[
          return states['light.test_light'].attributes
          && (states['light.test_light'].attributes.brightness <= 100)
        ]]]


      icon: mdi:alert
    - operator: default
      icon: mdi:lightbulb
- type: 'custom:button-card'
  color_type: icon
  entity: light.test_light
  show_label: true
  state:
    - operator: template
      value: >
        [[[ return states['input_select.light_mode'].state === 'night_mode' ]]]


      icon: mdi:weather-night
      label: Night Mode
    - operator: default
      icon: mdi:white-balance-sunny
      label: Day Mode
```

### Styling

![per-style-config](examples/per-style-config.gif)

```yaml
- type: 'custom:button-card'
  color_type: icon
  entity: light.test_light
  label: >
    [[[
      var bri = states['light.test_light'].attributes.brightness;
      return 'Brightness: ' + (bri ? bri : '0') + '%';
    ]]]


  show_label: true
  show_state: true
  size: 10%
  styles:
    card:
      - height: 100px
    label:
      - color: gray
      - font-size: 9px
      - justify-self: start
      - padding: 0px 5px
    name:
      - text-transform: uppercase
      - letter-spacing: 0.5em
      - font-familly: cursive
      - justify-self: start
      - padding: 0px 5px
    state:
      - justify-self: start
      - font-size: 10px
      - padding: 0px 5px
  state:
    - value: 'on'
      styles:
        state:
          - color: green
    - value: 'off'
      styles:
        state:
          - color: red
        card:
          - filter: brightness(40%)
- type: 'custom:button-card'
  color_type: icon
  entity: light.test_light
  layout: icon_label
  label: >
    [[[ return 'Other State: ' + states['switch.skylight'].state; ]]]


  show_label: true
  show_name: false
  size: 100%
  styles:
    card:
      - height: 200px
    label:
      - font-weight: bold
      - writing-mode: vertical-rl
      - text-orientation: mixed
  state:
    - value: 'on'
      styles:
        label:
          - color: red
    - value: 'off'
      styles:
        label:
          - color: green
```

### Lock

![lock-animation](examples/lock.gif)

```yaml
- type: horizontal-stack
  cards:
    - type: 'custom:button-card'
      entity: switch.test
      lock:
        enabled: true
    - type: 'custom:button-card'
      color_type: card
      lock:
        enabled: true
      color: black
      entity: switch.test
```

### Aspect Ratio

![aspect-ratio-image](examples/aspect_ratio.png)

```yaml
- type: vertical-stack
  cards:
    - type: horizontal-stack
      cards:
        - type: custom:button-card
          name: 1/1
          icon: mdi:lightbulb
          aspect_ratio: 1/1
        - type: custom:button-card
          name: 2/1
          icon: mdi:lightbulb
          aspect_ratio: 2/1
        - type: custom:button-card
          name: 3/1
          icon: mdi:lightbulb
          aspect_ratio: 3/1
        - type: custom:button-card
          name: 4/1
          icon: mdi:lightbulb
          aspect_ratio: 4/1
    - type: horizontal-stack
      cards:
        - type: custom:button-card
          name: 1/1.2
          icon: mdi:lightbulb
          aspect_ratio: 1/1.2
        - type: custom:button-card
          name: 1/1.3
          icon: mdi:lightbulb
          aspect_ratio: 1/1.3
        - type: custom:button-card
          name: 1/1.4
          icon: mdi:lightbulb
          aspect_ratio: 1/1.4
        - type: custom:button-card
          name: 1/1.5
          icon: mdi:lightbulb
          aspect_ratio: 1/1.5
```

### Changing the feedback color during a click

For dark cards, it can be usefull to change the feedback color when clicking the button. The ripple effect uses a `mwc-ripple` element so you can style it with the CSS variables it supports.

For example:

```yaml
styles:
  card:
    - --mdc-ripple-color: blue
    - --mdc-ripple-press-opacity: 0.5
```

## Community guides

- [robotnet.dk](https://robotnet.dk/2020/homekit-knapper-custom-buttons-home-assistant.html): Danish tutorial and how-to about using Lovelace Button card for your entities.

## Credits

- [ciotlosm](https://github.com/ciotlosm) for the readme template and some awesome examples
- [iantrich](https://github.com/iantrich), [LbDab](https://github.com/lbdab) and [jimz011](https://github.com/jimz011) for the inspiration and the awesome templates and cards you've created.
