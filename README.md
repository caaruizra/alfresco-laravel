

# Alfresco Client for Laravel
Access client to the Alfresco APIs (Rest and CMIS)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Install
```bash
composer require ajtarragona/alfresco-laravel:"@dev"
``` 

## Configuration
You can configure the package through the `.env` file of the application. These are the available parameters:

Parameter |  Descriptions  | Values
--- | --- | --- 
ALFRESCO_URL | API Base URL  | `http://ip_or_domain:port/alfresco/`
ALFRESCO_API | Api type | `rest` / <ins>`cmis`</ins> 
ALFRESCO_API_VERSION  | API Version Code | `1.0` (rest) / <ins>`1.1`</ins> (cmis)
ALFRESCO_REPOSITORY_ID  | Repository ID | `-default-` per defecte
ALFRESCO_BASE_ID  | Alfresco base directory ID |  
ALFRESCO_BASE_PATH  | Base directory path |  
ALFRESCO_USER  | Username |  
ALFRESCO_PASSWORD  | Password | --- 
ALFRESCO_DEBUG  | Enable debug | `true` / <ins>`false`</ins>
ALFRESCO_REPEATED_POLICY  | Overwrite policie of existing files | <ins>`rename`</ins> / `overwrite` / `deny`
ALFRESCO_EXPLORER  | Enable File Explorer | `true` / <ins>`false`</ins> 
ALFRESCO_VERIFY_SSL  | Enable SSL verification to Alfresco Server  | `true` / <ins>`false`</ins>  



Alternatively, you can publish the package configuration file with the command:

```bash
php artisan vendor:publish --tag=ajtarragona-alfresco
```

This will copy the `alfresco.php` file to the `config` folder.
 
## Usage
Once configured, the package is ready to use.
You can do it in the following ways


**Using `Facade`:**
```php
use Alfresco;
...
public  function  test(){
    $file=Alfresco::getDocument("xxx-yyy-zzz");
    ...
}
```

For Laravel < 5.6, you need to register the alias of the Facade in the file `config/app.php` :
 
```php
'aliases'  =>  [
    ...
    'Alfresco'  =>  Ajtarragona\AlfrescoLaravel\Facades\Alfresco::class
]
```

  

**Using dependency injection:**
In your controllers, helpers, model:

```php
use Ajtarragona\AlfrescoLaravel\Models\AlfrescoService;
...

public  function  test(AlfrescoService  $client){
    $file=$client->getDocument("xxx-yyy-zzz");
    ...
}
```

** Using `helper`:**
```php
...
public  function  test(){
    $file=alfresco()->getDocument("xxx-yyy-zzz");
    ...
}
```  

## Functions

Function |Description | Parameter | Return | Exceptions
--- | --- | --- | --- | ---
**getBasepath** | Returns the root directory from which the other methods will run |  | `string` | 
**setBasepath** | Defines the root directory from which the other methods will run | `string:$path`|  
**getBaseFolder** | Returns the BaseFolder (the root directory from basepath, if defined)) | | `AlfrescoFolder` |	
**exists** | Returns true if an object with the passed ID exists | `string:$objectId` | `boolean` |
**existsPath** | Returns true if an object with the passed path exists  | `string:$objectPath` | `boolean` |
**getObject** | Returns the object of the passed ID | `string:$objectId` |  `AlfrescoObject` | 
**getObjectByPath** | Retorna un objecte amb el path passat | `string:$objectPath` | `AlfrescoObject` | 
**downloadObject** | Descarrega el contingut d'un objecte passant el seu ID | `string:$objectId`<br/> `boolean:$stream=false` | Binary Content |  `AlfrescoObjectNotFoundException`
**getFolder** | Retorna una carpeta d'Alfresco passant el seu ID | `string:$folderId` | `AlfrescoFolder` |  `AlfrescoObjectNotFoundException`
**getFolderByPath** | Retorna una carpeta d'Alfresco passant la seva ruta (a partir del basepath) | `string:$folderPath` | `AlfrescoFolder` |  `AlfrescoObjectNotFoundException`
**getParent** | Retorna la carpeta pare de l'objecte amb l'ID passat | `string:$objectId` | `AlfrescoFolder` |  `AlfrescoObjectNotFoundException`
**getChildren** | Retorna els fills d'una carpeta d'Alfresco passant el seu ID | `string:$folderId` | `AlfrescoFolder[]` |  `AlfrescoObjectNotFoundException`
**createFolder** | Crea una carpeta passant el seu nom dins la carpeta amb l'ID passat.<br>Retorna la carpeta creada | `string:$folderName`<br>`string:$parentId=null` | `AlfrescoFolder` |  `AlfrescoObjectNotFoundException`<br>`AlfrescoObjectAlreadyExistsException`
**getDocument** | Retorna un document d'Alfresco passant el seu ID | `string:$documentId` | `AlfrescoDocument` |  `AlfrescoObjectNotFoundException`
**getDocumentByPath** | Retorna un document d'Alfresco passant la seva ruta (a partir del basepath) | `string:$documentPath` | `AlfrescoDocument` |  `AlfrescoObjectNotFoundException`
**getDocumentContent** | Retorna el contingut binari d'un document d'Alfresco passant el seu ID | `string:$documentId` | Binary Content | 
**delete** | Elimina el document o carpeta d'Alfresco amb l'ID passat | `string:$objectId` | `boolean` |  `AlfrescoObjectNotFoundException`
**copy** | Copia el document o carpeta d'Alfresco amb l'ID passat dins de la carpeta amb l'ID passat. Retorna el nou objecte. | `string:$objectId`<br>`string:$folderId` |   `AlfrescoObject` | `AlfrescoObjectNotFoundException`
**copyByPath** | Copia el document o carpeta d'Alfresco amb l'ID passat dins de la carpeta amb la ruta passada (a partir del basepath). Retorna el nou objecte. | `string:$objectId` <br>`string:$folderPath` | `AlfrescoObject` |  `AlfrescoObjectNotFoundException`<br>`AlfrescoObjectAlreadyExistsException`
**move** | Mou el document o carpeta d'Alfresco amb l'ID passat dins de la carpeta amb l'ID passat. Retorna el nou objecte. | `string:$objectId` <br> `string:$folderId` | `AlfrescoObject`   |`AlfrescoObjectNotFoundException`<br>`AlfrescoObjectAlreadyExistsException`
**moveByPath** | Mou el document o carpeta d'Alfresco amb l'ID passat dins de la carpeta amb la ruta passada (a partir del basepath). Retorna el nou objecte. | `string:$objectId`<br>`string:$folderPath` | `AlfrescoObject`  |  `AlfrescoObjectNotFoundException`<br>`AlfrescoObjectAlreadyExistsException`
**rename** | Renombra el document o carpeta d'Alfresco amb l'ID passat amb un nou nom. Retorna el nou objecte. | `string:$objectId`<br>`string:$newName` | `AlfrescoObject`  |  `AlfrescoObjectNotFoundException`<br>`AlfrescoObjectAlreadyExistsException`
**createDocument** | Crea un nou document a Alfresco a partir del contingut binari a la carpeta pare amb l'ID passat | `string:$parentId`<br>`string:$filename`<br>`string:$filecontent` | `AlfrescoObject`  |  `AlfrescoObjectNotFoundException`<br>`AlfrescoObjectAlreadyExistsException`
**createDocumentByPath** | Crea un nou document a Alfresco a partir del contingut binari a la carpeta pare amb la ruta passada (a partir del basepath)| `string:$parentPath`<br>`string:$filename`<br>`string:$filecontent` | `AlfrescoObject`  |  `AlfrescoObjectNotFoundException`<br>`AlfrescoObjectAlreadyExistsException`
**upload** | Carrega un document a Alfresco a partir d'un objecte `UploadedFile` o un array d'aquests. Típicament s'utilitza des d'un controlador Laravel, recollint els arxius de la request que venen d'un formulari multipart | `string:$parentId`<br>`UploadedFile-UploadedFile[]:$documents`  |  `AlfrescoDocument` or `string` in case of error
**getSites** | Retorna todos los Sites de alfresco (como objetos AlfrescoFolder)|   |  `AlfrescoFolder[]`
**search** | Busca documents que continguin el text passat al nom o al contingut a partir de la carpeta amb l'ID passat o l'arrel| `string:$query`<br>`string:$folderId=null`<br>`boolean:$recursive:false`  |  `AlfrescoObject[]` | `AlfrescoObjectNotFoundException`
**searchByPath** | Busca documents que continguin el text passat al nom o al contingut a partir de la carpeta amb la ruta passada (a partir de la carpeta arrel o al basepath si està definit) | `string:$query`<br>`string:$folderPath=null`<br>`boolean:$recursive:false`  |  `AlfrescoObject[]` | `AlfrescoObjectNotFoundException`



<a name="explorador"></a>
## File Explorer
IF the Alfresco Debug is active: 
```
ALFRESCO_DEBUG = true 
```
in the `.env` file, we can access a *file-explorer* in the path:

`/ajtarragona/alfresco/explorer`



