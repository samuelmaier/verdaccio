---
id: plugin
title: "Plugin"
---
Verdaccio è un'applicazione estensibile. Si può espandere in molti modi, o con nuovi metodi di autenticazione, aggiungendo endpoint o utilizzando un archivio personalizzato.

> Se sei interessato a sviluppare il tuo plugin personale, leggi la sezione [sviluppo](dev-plugins.md).

## Utilizzo

### Installazione

```bash
$> npm install --global verdaccio-activedirectory
```

`verdaccio` essendo un fork di sinopia, ha compatibilità con le versioni precedenti e con plugin che sono compatibili con `sinopia@1.4.0`. In questo caso l'installazione è la stessa.

    $> npm install --global sinopia-memory
    

### Configurazione

Aprire il file `config.yaml` e aggiornare la sezione `auth` come segue:

La configurazione predefinita appare così, poiché usiamo un plugin `htpasswd` incorporato di default che si può disabilitare commentando le seguenti linee.

### Configurazione del plugin Auth

```yaml
 htpasswd:
    file: ./htpasswd
    #max_users: 1000
```

e sostituendo con (in caso si decida di utilizzare un plugin `ldap`.

```yaml
auth:
  activedirectory:
    url: "ldap://10.0.100.1"
    baseDN: 'dc=sample,dc=local'
    domainSuffix: 'sample.local'
```

#### Multiple Auth plugins

This is tecnically possible, making the plugin order important, as the credentials will be resolved in order.

```yaml
auth:
  htpasswd:
    file: ./htpasswd
    #max_users: 1000
  activedirectory:
    url: "ldap://10.0.100.1"
    baseDN: 'dc=sample,dc=local'
    domainSuffix: 'sample.local'
```

### Configurazione del Plugin Middleware

Questo è un esempio di come si configura un plugin middleware. Tutti i plugin middleware devono essere definiti nel namespace **middlewares**.

```yaml
middlewares:
  audit:
    enabled: true
```

> Si potrebbe seguire il [plugin audit middle](https://github.com/verdaccio/verdaccio-audit) come esempio di base.

### Configurazione del Plugin Store

Questo è un esempio di come configurare un plugin di archiviazione. Tutti i plugin di archiviazione devono essere definiti nel namespace **store**.

```yaml
store:
  memory:
    limit: 1000
```

> Se si definisce uno store personalizzato, la proprietà di **archiviazione** nel file di configurazione verrà ignorata.

## Plugin ereditati

### Plugin di Sinopia

(compatibili con tutte le versioni)

* [sinopia-npm](https://www.npmjs.com/package/sinopia-npm): plugin auth per il supporto di sinopia a un registro npm.
* [sinopia-memory](https://www.npmjs.com/package/sinopia-memory): plugin auth per sinopia che mantiene gli utenti in memoria.
* [sinopia-github-oauth-cli](https://www.npmjs.com/package/sinopia-github-oauth-cli).
* [sinopia-crowd](https://www.npmjs.com/package/sinopia-crowd): auth plugin for sinopia supporting atlassian crowd.
* [sinopia-activedirectory](https://www.npmjs.com/package/sinopia-activedirectory): Active Directory authentication plugin for sinopia.
* [sinopia-github-oauth](https://www.npmjs.com/package/sinopia-github-oauth): authentication plugin for sinopia2, supporting github oauth web flow.
* [sinopia-delegated-auth](https://www.npmjs.com/package/sinopia-delegated-auth): Sinopia authentication plugin that delegates authentication to another HTTP URL
* [sinopia-altldap](https://www.npmjs.com/package/sinopia-altldap): Alternate LDAP Auth plugin for Sinopia
* [sinopia-request](https://www.npmjs.com/package/sinopia-request): An easy and fully auth-plugin with configuration to use an external API.
* [sinopia-htaccess-gpg-email](https://www.npmjs.com/package/sinopia-htaccess-gpg-email): Generate password in htaccess format, encrypt with GPG and send via MailGun API to users.
* [sinopia-mongodb](https://www.npmjs.com/package/sinopia-mongodb): An easy and fully auth-plugin with configuration to use a mongodb database.
* [sinopia-htpasswd](https://www.npmjs.com/package/sinopia-htpasswd): auth plugin for sinopia supporting htpasswd format.
* [sinopia-leveldb](https://www.npmjs.com/package/sinopia-leveldb): a leveldb backed auth plugin for sinopia private npm.
* [sinopia-gitlabheres](https://www.npmjs.com/package/sinopia-gitlabheres): Gitlab authentication plugin for sinopia.
* [sinopia-gitlab](https://www.npmjs.com/package/sinopia-gitlab): Gitlab authentication plugin for sinopia
* [sinopia-ldap](https://www.npmjs.com/package/sinopia-ldap): LDAP auth plugin for sinopia.
* [sinopia-github-oauth-env](https://www.npmjs.com/package/sinopia-github-oauth-env) Sinopia authentication plugin with github oauth web flow.

> All sinopia plugins should be compatible with all future verdaccio versions. Anyhow, we encourage contributors to migrate them to the modern verdaccio API and using the prefix as *verdaccio-xx-name*.

## Verdaccio Plugins

(compatible since 2.1.x)

### Authorization Plugins

* [verdaccio-bitbucket](https://github.com/idangozlan/verdaccio-bitbucket): Bitbucket authentication plugin for verdaccio.
* [verdaccio-ldap](https://www.npmjs.com/package/verdaccio-ldap): LDAP auth plugin for verdaccio.
* [verdaccio-active-directory](https://github.com/nowhammies/verdaccio-activedirectory): Active Directory authentication plugin for verdaccio
* [verdaccio-gitlab](https://github.com/bufferoverflow/verdaccio-gitlab): use GitLab Personal Access Token to authenticate
* [verdaccio-htpasswd](https://github.com/verdaccio/verdaccio-htpasswd): Auth based on htpasswd file plugin (built-in) for verdaccio
* [verdaccio-github-oauth](https://github.com/aroundus-inc/verdaccio-github-oauth): Github oauth authentication plugin for verdaccio.
* [verdaccio-github-oauth-ui](https://github.com/n4bb12/verdaccio-github-oauth-ui): GitHub OAuth plugin for the verdaccio login button.

### Middleware Plugins

* [verdaccio-audit](https://github.com/verdaccio/verdaccio-audit): verdaccio plugin for *npm audit* cli support (built-in) (compatible since 3.x)

* [verdaccio-profile-api](https://github.com/ahoracek/verdaccio-profile-api): verdacci plugin for *npm profile* cli support and *npm profile set password* for *verdaccio-htpasswd* based authentificaton

### Storage Plugins

(compatible since 3.x)

* [verdaccio-memory](https://github.com/verdaccio/verdaccio-memory) Storage plugin to host packages in Memory
* [verdaccio-s3-storage](https://github.com/remitly/verdaccio-s3-storage) Storage plugin to host packages **Amazon S3**
* [verdaccio-google-cloud](https://github.com/verdaccio/verdaccio-google-cloud) Storage plugin to host packages **Google Cloud Storage**

## Caveats

> Not all these plugins are been tested continuously, some of them might not work at all. Please if you found any issue feel free to notify the owner of each plugin.