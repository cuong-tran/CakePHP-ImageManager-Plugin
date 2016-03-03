CakePHP-ImageManager-Plugin
===========================

## Setting up

### Using Git
#### Put this into your Cake's plugin directory
1. Go to your app/Plugin
2. Donwload the whole source into it
3. Or using git:
```bash
git clone https://github.com/cuong-tran/CakePHP-ImageManager-Plugin.git ImageManager
```
4. Change name to ImageManager if not done yet

#### Requirement:
- Bootstrap: Put this in your Layout *<head></head>*
```html
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" integrity="sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7" crossorigin="anonymous">

<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap-theme.min.css" integrity="sha384-fLW2N01lMqjakBkx3l/M9EahuwpSfeNvV63J5ezn3uZzapT0u7EYsXMjQV+0En5r" crossorigin="anonymous">

<!-- Latest compiled and minified JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" integrity="sha384-0mSbJDEHialfmuBBQP6A4Qrprq5OVfW37PRR3j5ELqxss1yVqOtnepnHVP9aJ7xS" crossorigin="anonymous"></script>
```
 - jQuery: Put this in your Layout *<head></head>*
```html
<script src="http://code.jquery.com/jquery-1.12.0.min.js"></script>
<script src="http://code.jquery.com/jquery-migrate-1.2.1.min.js"></script>
```

### In your model

        public $actsAs = array('ImageManager.ImageManager');

        // Gallery
        public $hasAndBelongsToMany = array(
                'Image' => array(
                    'className' => 'ImageManager.Image',
                    'foreignKey' => 'foreign_id',
                    'associationForeignKey' => 'image_id',
                    'joinTable' => 'images_relations',
                    'conditions' => array('foreign_name' => 'Page')
                ),
            );

        // One image
        // Name of association should by 'ModelField'
        public $belongsTo = array(
                'PageImageId' => array(
                    'className' => 'ImageManager.Image',
                    'foreignKey' => 'image_id',
                )
            );


### In your layout

        echo $this->Html->script(Router::url(array(
                'controller'=> 'images',
                'action' => 'scripts',
                'full_base' => true,
                'admin' => false,
                'plugin' => 'image_manager',
        )));
        echo $this->Html->script('ImageManager.image_manager');
        echo $this->Html->script('ImageManager.jquery.drag.drop');
        echo $this->Html->css('ImageManager.image_manager');


### In your config file

        Configure::write(
            array (
                'ImageManager.Upload' => array(
                    'filename' => array(
                        'thumbnailSizes' => array(
                            'small' => '500x500',
                            'thumb' => '253x158',
                            'big' => '800l',
                            'nano' => '100x100',
                            'pico' => '70x70',
                        )
                    ),
                )
            )
        );


### In your controller

        public $helpers = array(
                'ImageManager.ImageManager'
            );


### Run MySQL script

        CREATE TABLE `images` (
              `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
              `foreign_key` int(10) unsigned DEFAULT NULL,
              `model` varchar(255) NOT NULL DEFAULT '',
              `filename` varchar(255) NOT NULL DEFAULT '',
              `dir` int(11) unsigned DEFAULT NULL,
              `order` int(11) unsigned DEFAULT NULL,
              `is_slider` int(2) unsigned DEFAULT NULL,
              `site_id` int(11) unsigned DEFAULT NULL,
              PRIMARY KEY (`id`)
        ) ENGINE=MyISAM  DEFAULT CHARSET=utf8;

        CREATE TABLE `images_relations` (
              `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
              `foreign_id` int(11) unsigned NOT NULL,
              `foreign_name` varchar(255) NOT NULL,
              `image_id` int(11) unsigned NOT NULL,
              PRIMARY KEY (`id`)
        ) ENGINE=MyISAM  DEFAULT CHARSET=utf8;


### Usage
    // One image
    <?php echo $this->ImageManager->image('Page.image_id'); ?>

    // Gallary
    <?php echo $this->ImageManager->gallery('Gallery'); ?>
