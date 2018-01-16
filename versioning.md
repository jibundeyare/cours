# Versionning

Avec le versionning sémantique, chaque nombre signifie quelque chose dans le numéro de version.

Prenons par exemple `3.4.2` :

- `3` est le numéro de version majeur
- `4` est le numéro de version mineur
- `2` est le numéro de version de patch

## Numéro de version majeur

Un changement de numéro de version majeur indique que le code apporte des changements incompatibles avec la version précédente. On parle de changement non rétro-compatible (ou BC Break en anglais).

Par exemple, la version `3.0.0` n'est pas compatible avec la version `2.8.32`.

# Numéro de version mineur

Un changement de numéro de version mineur indique que le code apporte des améliorations compatibles avec la version précédente. On parle de changement rétro-compatible (backward compatible en anglais).

Par exemple, la version `3.4.2` apporte des fonctionnalités supplémentaire à la version `3.3.14`.

## Numéro de patch

Un changement de numéro de version de patch indique que le code apporte des corrections à la version précédente.

Par exemple, la version `3.4.2` apporte des corrections à la version `3.3.1`.
