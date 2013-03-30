This cookbook entry demonstrates how to use the single file upload feature. We suppose you already have the necessary bundles (VichUploaderBundle, GaufretteBundle and ImagineBundle) in your vendor folder.

In this example we will create a new entity called Product with some basic fields including the name of the image that contains a picture of the product. Using the generated admin interface the user will be able to upload a new image and assign it to the product.

### 1. Basic configuration
[VichUploaderBundle](https://github.com/dustin10/VichUploaderBundle) requires some basic configration steps. As we want to keep this example simple we will use the simplest setup with Doctrine ORM used for entities and local file system for storing image files. For advanced settings see the [official documentation](https://github.com/dustin10/VichUploaderBundle/blob/master/Resources/doc/index.md).

Add the following configuration to the `app/config.yml` file:
```yaml
vich_uploader:
    db_driver: orm
    mappings:
        product_image:
            upload_destination: %kernel.root_dir%/../web/images/product_images
```

* Create a new bundle called `Acme/ProductBundle` using the `generate:bundle` command
* Generate a single entity called AcmeProductBundle:Product using the `doctrine:generate:entity` command. Create three new fields:
   * name string 255

