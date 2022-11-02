
# Utiliser le bundle FOSCKEditor dans une application Symfony 6

Comment utiliser le bundle FOSCKEditor dans une application Symfony 6.


*Pré-requis* : 
- Avoir créé une application Symfony 6
- Avoir installé le bundle Webpack Encore

## Les différentes étapes 

1. **Installer le bundle FOSCKEditor**
```
$ composer require friendsofsymfony/ckeditor-bundle
```
```
npm install –save ckeditor4@^4.13.0
```
Cela ajoute le bundle à *node_modules* lorsque l'on utilise Webpack Encore

2. **Modifier le fichier webpack.config.js**
```javascript
Encore
    // ...
    .copyFiles([
        {from: './node_modules/ckeditor4/', to: 'ckeditor/[path][name].[ext]', pattern: /\.(js|css)$/, includeSubdirectories: false},
        {from: './node_modules/ckeditor4/adapters', to: 'ckeditor/adapters/[path][name].[ext]'},
        {from: './node_modules/ckeditor4/lang', to: 'ckeditor/lang/[path][name].[ext]'},
        {from: './node_modules/ckeditor4/plugins', to: 'ckeditor/plugins/[path][name].[ext]'},
        {from: './node_modules/ckeditor4/skins', to: 'ckeditor/skins/[path][name].[ext]'},
        {from: './node_modules/ckeditor4/vendor', to: 'ckeditor/vendor/[path][name].[ext]'}
    ])
;
```

3. **Modifier le fichier *fos_ckeditor.yaml***
Ce fichier a été créé au moment de l’installation du bundle dans le dossier *config/packages/*
```yaml
fos_ck_editor:
    #...
    base_path: "build/ckeditor"
    js_path: "build/ckeditor/ckeditor.js"
    configs:
        my_config:
            toolbar: full #peut-être full, standard ou basic
            language: fr
            extraPlugins: 'justify' #optionnel
```

4. **Lancer la commande**
```
$ npm run watch
```

5. **Utiliser le bundle dans Easyadmin**
- Configurer ses CRUDController
Ajouter la fonction suivante :
```php
public function configureCrud(Crud $scrud) : Crud
{
    return $crud
        ->addFormTheme(@FOSCKEditor/Form/ckeditor_widget.html.twig');
}
```
- Appliquer le CKEditorType au champ concerné
```php
TextareaField::new('champs')->setFormType(CKEditorType::class),
```
- Penser à importer la classe
```php
use FOS\CKEditorBundle\Form\Type\CKEditorType;
```

## Documentation Symfony

- <https://symfony.com/bundles/FOSCKEditorBundle/current/index.html#overview>