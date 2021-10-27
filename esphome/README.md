## Configuration du firmware ESPHome ##
Créez le fichier `secrets.yaml` dans le même répertoire que `teleinfokit.yaml` pour initialiser les valeurs suivantes :
* `wifi_ssid` : le nom de votre réseau wi-fi
* `wifi_key` : votre clé wi-fi
* `ap_key` : la clé souhaitée pour le point d'accès qui sera créé si le Teleinfokit ne peut se connecter à votre réseau wi-fi
* `api_pass` : le mot de passe souhaité pour utiliser le TeleInfoKit avec Home-Assistant
* `ota_pass` : le mot de passe pour les mise à jour via wi-fi

### Notes ###
- Une chaîne vide (`""`) permet de ne mettre aucune valeur.
  Exemple : `api_pass : ""` permet de ne mettre aucun mot de passe pour l'accès via Home-Assistant.
- Si vous souhaitez utiliser le caractère "\", vous devez en écrire deux.
  Ex : `"mon\\passe"`

## exemple de fichier `secrets.yaml` ##
```yaml
wifi_ssid: "Mon_wifi"
wifi_key: "La clé de mon wi-Fi123"
ap_key: "givememydata"
api_pass: "Welcome_HA"
ota_pass: "passe_MAJ"
```

## Changer le mot de passe OTA ##
Il n'est pas possible de changer tout simplement le mot de passe dans le fichier secrets.yaml et lancer la mise à jour puisque le processus de mise à jour OTA se sert du mot de passe configuré pour pouvoir mettre à jour le TeleInfoKit.
Suivez les instruction de [la documentation ESPHome](https://www.esphome.io/components/ota.html#updating-the-password) pour pouvoir changer le mot de passe OTA via une mise à jour OTA.
Pour ceux qui préfèrent des instructions en français :
Dans le bloc `esphome`, rajouter :
```yaml
  on_boot:
    - lambda: |-
        id(my_ota).set_auth_password("New password");
```

Mettez en suite à jour le TeleInfoKit.
Une fois la mise à jour OTA terminée, supprimez les lignes ajoutées précédemment et changez la valeur de `ota_pass` dans votre fichier secrets.yaml.

## Configuration avec des fonctionnalités hardware supplémentaires

Une version du firmware avec un capteur de température/pression de type BMP280 sur le même bus I2C que l'écran est disponible également dans ce dossier (`teleinfokit_bmp.yml`). Le capteur est connecté via les pins déportés sur le bord du circuit (SDA : `GPIO0` et SCL : `GPIO2`) pour le bus I2C et l'alimentation par le 3V3 également accessible.

Ce n'est qu'un exemple de ce qui peut être rajouté à l'aide du bus I2C.

## écran éteint par défaut

Par défaut, l'écran du est allumé dès le démarrage.
La variable de substitution `off_on_boot` permet de définir si l'écran est éteint par défaut.
Pour cela, il suffit de placer sa valeur à `"true`"

### En ligne de commande

Dans le répertoire où se trouve teleinfokit.yml :
`esphome -s off_on_boot true teleinfokit.yml`

### alternative YAML

Grâce aux packages ESPHome, il est possible d'avoir l'écran éteint par défaut.
Pour cela, dans le répertoire où se trouve le fichier teleinfokit.yml, créez un autre fichier yml ou yaml, par exemple main.yaml avec le contenu suivant :
```yaml
substitutions:
  off_on_boot: 'true'

packages:
  teleinfokit: !include teleinfokit.yml
```
