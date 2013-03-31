This cookbook entry demonstrates how to use the single file upload feature. We suppose you already have the required bundles (VichUploaderBundle, GaufretteBundle and ImagineBundle) in your vendor folder and they have been initialized in `app/AppKernel.php` file.

In this example we will create a new entity called Product with some basic fields including the name of the image that contains a picture of the product. Using the generated admin interface the user will be able to upload a new image and assign it to the product.

### 1. Basic configuration
[VichUploaderBundle](https://github.com/dustin10/VichUploaderBundle) requires some basic configration steps. We want to keep this example simple so we will use a very simplistic setup with Doctrine ORM used for managing entities and local file system for storing image files. For advanced configuration options see the [official documentation](https://github.com/dustin10/VichUploaderBundle/blob/master/Resources/doc/index.md).

Add the following configuration to the `app/config.yml` file. This creates a mapping for `product_image` and specifies the destination directory where images will be stored.
```yaml
vich_uploader:
    db_driver: orm
    mappings:
        product_image:
            upload_destination: %kernel.root_dir%/../web/images/products
```

Add the following line to the `admingenerator_generator` section:
```yaml
admingenerator_generator:
    thumbnail_generator:    avalanche
```

### 2. Generating the bundle and the Product entity
First, create a new bundle called `Acme/ProductBundle` using the `generate:bundle` command. 
Then, generate a single entity called AcmeProductBundle:Product using the `doctrine:generate:entity` command. Create three new fields:
   * **name:** name **type:** string **length:** 255
   * **name:** price **type:** integer
   * **name:** imageName **type:** string **length:** 255

Entity will be generated in `src/Acme/ProductBundle/Entity/Product.php`.

### 3. Annotating the Product entity
* Open the generated ``Product.php`` file and add few annotations to it. 

```php
//...
namespace Acme\ProductBundle\Entity;

use Doctrine\ORM\Mapping as ORM;
use Symfony\Component\Validator\Constraints as Assert;
use Vich\UploaderBundle\Mapping\Annotation as Vich;

/**
 * Product
 *
 * @ORM\Table()
 * @ORM\Entity
 * @Vich\Uploadable
 */
 //...
 ```
 
 Insert the following field definition somewhere in the Product entity:
 
```php
//...
    /**
     * @Assert\File(
     *     maxSize="10M",
     *     mimeTypes={"image/png", "image/jpeg", "image/pjpeg"}
     * )
     * @Vich\UploadableField(mapping="product_image", fileNameProperty="imageName")
     *
     * @var File $image
     */
    protected $image;    
//...
```

Create or update the database schema using the doctrine:schema commands.

### 4. Running admin:generate-admin
Run the `admin:generate-admin` command to generate admin GUI in the existing AcmeProductBundle bundle with the following parameters:
* **Bundle namespace:** Acme\ProductBundle
* **Model name [YourModel]:** Product
* **Bundle name [AcmeProductBundle]:** accept default value
* **Target directory:** accept default value
* **Prefix of yaml:** product

Let the generator update your routing.yml file so that the auto-generated admin interface will be available at the `/admin/acme_product_bundle/product/` URL. Visit the URL and try adding a new product. You can only manually enter the file name but no upload widget is available. Actually, it will take few additional steps to make it work.

### 5. Customizing generator.yml
Generator created a new YAML file called `Product-generator.yml` in the `src/Acme/ProductBundle/Resources/config` directory. This is the configuration file of the admin generator that needs to be further customized.
