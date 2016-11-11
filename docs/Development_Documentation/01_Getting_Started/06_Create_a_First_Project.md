
In this part, you are going to known required minimum of knowledge, crucial for start developing with Pimcore. 

[TOC]

# Creating CMS Pages with Documents

In the first part you'll learn the basics for creating CMS pages with Pimcore Documents. 

## Create Template, Layout and Controller

### New Controller
First of all, we need our own controller. 
Let's call it `ContentController.php`. 
You have to put the file into the `/website/controllers` directory.

```php
<?php

use Website\Controller\Action;
use Pimcore\Model\Asset;

class ContentController extends Action
{

    public function defaultAction()
    {
        $this->enableLayout();
    }
}
```

There is the only one action `defaultAction()`.
The method `enableLayout()` registers a global instance of `\Zend_Layout` to decorate the page. 
In the defaultAction, we can put some custom code or assign values to the template.

### Create a Template
Now we create a template for our page:
* Create a new folder in `/website/view/scripts` and name it like the controller but in lowercase (in this case `content`). 
* Put a new PHP file into this folder and name it like our action, again in lowercase (`default.php`).
* If your're using camel-case for the name of your action/controller, it's still lowercase but separated by a hyphen: `myCustomAction` => `my-custom.php` ...

Then we can put some template code into it, for example:

```php
<?php /** @var $this \Pimcore\View */ ?>

<?php $this->layout()->setLayout('default'); ?>

    <h1><?= $this->input("headline", ["width" => 540]); ?></h1>

<?php while ($this->block("contentblock")->loop()) { ?>
    <h2><?= $this->input("subline"); ?></h2>
    <?= $this->wysiwyg("content"); ?>
<?php } ?>
```

Pimcore uses `\Zend_View` as templates and therefore plain php as template language. So you have the full power of
  `\Zend_View` with all ZF (*Zend Framework*) functionality available. In addition to that, there are some Pimcore additions like so called *editables*,
  which add editable parts (placeholders) to the layout. 
  For details concerning editables (like `$this->input`, `$this->block`, ...) see [Editables](../03_Documents/01_Editables/README.md). 

### Add a Layout
Pimcore uses the advantages of `\Zend_Layout` out of the ZF, for details please read more [here](https://framework.zend.com/manual/1.12/en/zend.layout.html) about it.
Because we have enabled the layout engine in our controller, we can use layouts to wrap our content page with another template which contains the main navigation, a sidebar, …
using the following code:

```php
<?php $this->layout()->setLayout('default'); ?>
```
We tell the engine that we want to use the layout `default`. Now create a new php file in the folder `/website/views/layouts` and name it to default.php (just like the name of the layout appending .php).
Then we can also put some HTML and template code into it:

```php
<?php /** @var $this \Pimcore\View */ ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title>Example</title>
    <link rel="stylesheet" type="text/css" href="/website/static/css/screen.css" />
</head>
<body>
    <div id="site">
        <div id="logo">
            <a href="http://www.pimcore.org/"><img src="/pimcore/static6/img/logo-gray.svg" style="width: 200px;" /></a>
            <hr />
            <div class="claim">
                THE OPEN-SOURCE ENTERPRISE PLATFORM FOR PIM, CMS, DAM & COMMERCE
            </div>
        </div>
        <div class="info">
            <?= $this->layout()->content; ?>
        </div>
    </div>
</body>
</html>
```

The code `<?= $this->layout()->content ?>` is the placeholder where your contentpage will be inserted.

### Putting It Together with Pimcore Documents
Now we need to connect the action to a page. This is done in the Pimcore backend.
First, click right on *Home* in the *Documents* panel. 

![Create page](../img/Pimcore_Elements_check_homepage.png)

Now select the tab *Settings* in the newly opened tab.
Select the name of the controller and the name of the action in the accordingly fields.

![Page settings](../img/Pimcore_Elements_homepage_settings.png)

You can test the new controller and action, after saving the document (press *Save & Publish*).
Select the tab *Edit*, to see your page with all the editable placeholders.

![Page edit preview](../img/Pimcore_Elements_homepage_edit_tab.png)


# Introduction to Assets

In assets, all binary files like images, videos, office files and PDFs, ... can be uploaded, stored and managed. 
You can organize them in a directory structure and assign them additional meta data. 
Once uploaded, an asset can be used and linked in multiple places - e.g. documents or objects. 

In terms of images or videos, always upload only one high quality version (best quality available). 
[Thumbnails](../04_Assets/03_Working_with_Thumbnails/01_Image_Thumbnails.md) for different output channels are created directly [within Pimcore](../04_Assets/03_Working_with_Thumbnails/01_Image_Thumbnails.md) using custom configurations.

For this tutorial, at least add one file which  you will use in an object later. 

There are many ways to upload files:
1. Drag & drop files from your file explorer into the browser on the desired asset folder
2. Right click on *Home* and choose the most suitable method for you

![Upload assets](../img/asset-upload.png)


# Introduction to Objects
We've already made a controller, action and a view so we're able to add text from within the admin panel to our pages.
In this chapter we will create a simple product database and use them in our CMS pages. 
Objects are used to store any structured data independently from the output-channel and can be used anywhere in your project. 

## Create the Class Model/Definition

Ok, let's create our first class for objects. 

Go to: *Settings -> Object -> Classes* and click the button *Add Class*.

![Add product class](../img/Pimcore_Elements_class_add.png)

Now, there is a new product class/model which is a representation of your entity including the underlying database 
scheme as well as a generated PHP class you can use to create, update, list and delete your entities. 

More specific backgrounds and insights can be found in the [Objects section](../05_Objects/README.md)

The product should have the following attributes: **SKU**, **picture**, **name** and **description**. 
To add them follow these steps: 

* Go to the edit page of the class product 
* Click right on *Base* and select *Add Layout Component -> Panel* - This is the main panel/container for the following product attributes
* To add attributes:
    * Click right on *Panel*, then *Add data component -> Text -> Input*, then change the name of the input component to **sku** (in the edit panel on the right side)
    * Just the same way you add the new data field for **name**
    * Now we're going to add a WYSIWYG attribute for the **description**. Again, click right, select *Add data component -> Text -> WYSIWYG*. We name it *description*.
    * The last attribute is for the picture. We can use on of the specialized image components in *Other -> Image*. Name the attribute **picture**.

If everything goes well, the new class looks like in the picture:

![Product class](../img/Pimcore_Elements_product_class.png)

**Important:** Every generated class in the Pimcore admin panel has also an accordingly PHP class with getters and setters. You can find our newly created class above class in `website/var/classes/Object/Product.php` 

## Add a new Object

We've just prepared a simple class for new products. 
Now we can use it to create objects in Pimcore.

* Open the objects section on the left and click on the right button after *Home* (Note that you can create also directories structure for objects).
* Choose *Add object -> product* and fill the input with a name, for example: *tshirt*
* Add values for sku, name and description attributes.
* Click  *Save & Publish*

Probably, your view looks like below:

![New product](../img/Pimcore_Elements_new_product.png)

The last step to finish the product object is add a photo.

The one way to upload a photo is this button: ![Upload image to an object](../img/Pimcore_Elements_upload_button.png) or just drag file which you uploaded from Assets section.

Click *Save & Publish* button. 

That's it. 

![Complete object](../img/Pimcore_Elements_complete_object.png)


# Putting the Pieces Together
Let's put the pieces together and connect the products to the CMS. 

## Update Controller and Template
Therefore create another action in the controller (ContentController) called `productAction`.
 
```php
<?php

use Website\Controller\Action;
use Pimcore\Model\Asset;

class ContentController extends Action
{
    public function defaultAction ()
    {
        $this->enableLayout();
    }
    
    public function productAction()
    {
        $this->enableLayout();
    }
}
```

Then we also need a new template: `website/views/scripts/content/product.php` 

```php
<?php /** @var $this \Pimcore\View */ ?>
<?php $this->layout()->setLayout('default'); ?>

<h1><?= $this->input("headline", ["width" => 540]); ?></h1>

<div class="product-info">
    <?php if($this->editmode):
        echo $this->href('product');
    else: ?>

<!-- Product information-->

    <?php endif; ?>
</div>

```

`$this->editmode` is a standard variable (is always set) to check if the view is called from the Pimcore admin backend and gives you the 
 possibility to to different stuff in editmode and in frontend. 

`$this->href('product')` is one of the possible editable placeholders. It can be used to make 1 to 1 relations, a cool alternative for that, would be also the [Renderlet](../03_Documents/01_Editables/28_Renderlet.md) editable.  
Click [here](../03_Documents/01_Editables/README.md) for a full list of available editables in Pimcore.


## Add the Product Object to a Document

The last thing is to show the product in the body of the document you created. 

Let's go back to the documents section. Right click on *Home* then *Add Page* > *Empty Page*.
In the settings label, choose the `product` action and the `content` controller, click save and go back to the edit tab.

Now you can see the new editable element (`href`) which we added in the product template above.
Drag the product object to that editable and press *Save & Publish*.

![Drag the object to the document](../img/Pimcore_Elements_drag_to_document.png)

Let's see what happened on the frontend... 

Go to the product page. In my case, let's say `http://pimcore.local/tshirt` where *tshirt* is the identifier of the product (the name visible the documents tree).

We haven't implemented frontend features yet, therefore the page doesn't contain any product information.

In the template file (`website/views/scripts/content/product.php`) add few lines:

```php
<?php /** @var $this \Pimcore\View */ ?>
<?php $this->layout()->setLayout('default'); ?>

<h1><?= $this->input("headline", ["width" => 540]); ?></h1>
<div class="product-info">
    <?php if($this->editmode):
        echo $this->href('product');
    else: ?>
    <div id="product">
        <?php
        /** @var \Pimcore\Model\Object\Product $product */
        $product = $this->href('product')->getElement();
        ?>
        <h2><?= $this->escape($product->getName()); ?></h2>
        <div class="content">
            <?= $product->getDescription(); ?>
        </div>
    </div>
    <?php endif; ?>
</div>

```

You are now able to access the linked object above by using the method `getElement()`.
Now you have access to the entire data from the linked object (name, description, ...).
It's a good practice to add `@var` PHPDoc in every view, to benefit from auto-complete in your IDE.

## Add a Thumbnail Configuration
To show the product image in the view, we need to add a thumbnail configuration first. Using [thumbnail configurations](../04_Assets/03_Working_with_Thumbnails/01_Image_Thumbnails.md),
Pimcore automatically renders optimized images for certain output channels (including high-res @2x versions). 

For adding a thumbnail configuration see the following screen. Just add a configuration named `content`. 
![Adding thumbnail configuration](../img/adding_thumbnails.png)


## Showing the Image in the View
Now, last but not least, we would like to show the product picture: 

```php
<div class="content">
    <?php
    $picture = $product->getPicture();
    if($picture instanceof \Pimcore\Model\Asset\Image):
        /** @var \Pimcore\Model\Asset\Image $picture */
        
    ?>
        <?= $picture->getThumbnail("content")->getHTML(); ?>
    <?php endif; ?>
    <?= $product->getDescription(); ?>
</div>
```
As you can see, image attribute is an additional class with useful parameter.
To print out the image in the right size just use the method `getThumbnail()->getHTML()` which returns the `<img>` or `<picture>` (when using media queries in your config) tag with the 
correct image path and also sets alt attributes to values based on the asset meta data. 

Now the product page should look like:

![Final product page](../img/Pimcore_Elements_final_product_page.png)