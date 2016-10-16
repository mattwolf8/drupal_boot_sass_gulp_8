# drupal_boot_sass_gulp_8

## Create a Drupal 8 Bootstrap SubTheme with Sass and gulp

####GET BOOTSTRAP COMPONENTS FOR DRUPAL
  Download and Install the Bootstrap Theme

    https://www.drupal.org/project/bootstrap

####CREATE THE SUBTHEME DIRECTORY
  Copy the Sass directory

    drupal/public/themes/bootstrap/starterkits/sass

  Paste the Sass directory

    public/themes/

  Rename the Sass directory

    sites/all/themes/bootstrap_subtheme

  Open the bootstrap_subtheme directory

  Rename:
  THEMENAME.info.yml
  THEMENAME.libraries.yml
  THEMENAME.theme
  config/install/THEMENAME.settings.yml
  config/schema/THEMENAME.schema.yml

  To:
  bootstrap_subtheme.info.yml
  bootstrap_subtheme.libraries.yml
  bootstrap_subtheme.theme
  config/install/bootstrap_subtheme.settings.yml
  config/schema/bootstrap_subtheme.schema.yml


  bootstrap_subtheme.info.yml

  Change the name to bootstrap_subtheme

  Change the description and any other properties to suite your needs.

  Change Default theme to sub_theme in appearances

#### Activate the subtheme directory

On your site, Clear all caches
Navigate to admin/appearance on your site
At the bottom, the new bootstrap_subtheme will appearance
Press Install and Set as default
Now bootstrap_subtheme is default, and you may click settings to customize default bootstrap behaivor.

#### GET BOOTSTRAP FOR SASS

Add the following folder structure to bootstrap_subtheme folder

* assets  Will hold all your new editables
* assets/images  Will hold the files to be turned into sprites using
* assets/sass  Will hold all of your custom SCSS files
* bootstrap  Will carry all of the Bootstrap SASS source
* img  Will hold your compiled images/sprites
* js  Carries your compiled JavaScript
* fonts

Download a copy of Bootstrap SASS
https://github.com/twbs/bootstrap-sass

Unzip downloaded file and navigate to assets folder.

Copy fonts from the assets folder and paste to:
bootstrap_subtheme/

Copy everything but fonts from the assets folder and paste to:
bootstrap_subtheme/bootstrap

In the bootstrap folder you just created, you’ll want to retrieve two files and copy them to your assets/sass folder:

* drupalroot/themes/mytheme/bootstrap/stylesheets/_bootstrap.scss

TO
* drupalroot/themes/mytheme/assets/sass/_bootstrap.scss

AND

* drupalroot/themes/mytheme/bootstrap/stylesheets/bootstrap/_variables.scss

TO
* drupalroot/themes/mytheme/assets/sass/_variables.scss

Now, rename
* assets/sass/_bootstrap.scss

TO
* assets/sass/style.scss

Open assets/sass/style.scss in a text editor and use Find and Replace
to change all instances of
* "bootstrap/"

TO
* “../../bootstrap/stylesheets/bootstrap/”

Now change
* @import "../../bootstrap/stylesheets/bootstrap/variables";

TO
* @import "variables";


####SETTING UP GULP
  In terminal, navigate to the sub_theme directory

    $ public/themes/bootstrap_subtheme

  Initialize NPM

    $ npm init

    (enter through the prompts)

  Open package.json with IDE

    /sub_theme/package.json

  Change the scripts object from:

    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },  

  Change the scripts object to:

    "scripts": {
      "postinstall": "find node_modules/ -name '*.info' -type f -delete"
    },

  In terminal, navigate to the sub_theme directory

    $ sites/all/themes/sub_theme

  create a file called .npmrc

    $ touch .npmrc

  open .npmrc

    $ nano .npmrc

  add the following

    $ unsafe-perm = true

  Save and Exit Nano

    $ ^x
    $ Y

  In terminal, navigate to the sub_theme directory

    $ sites/all/themes/sub_theme

  install gulp
    $ npm install gulp --save-dev

  install gup-sass
    $ npm install gulp-sass --save-dev

  install gulp-autoprefixer
    $ npm install --save-dev gulp-autoprefixer

  In the subtheme director, create the gulpfile.js

    $ touch gulpfile.js

  In an IDE open gulpfile.js and add the following:

  var gulp = require('gulp');
  var sass = require('gulp-sass');
  var prefix = require('gulp-autoprefixer');

  gulp.task('hello', function() {
    console.log('Hello sub_theme builder');
  });

  gulp.task('sass', function(){
    return gulp.src('./assets/sass/style.scss')
      // Converts Sass to CSS with gulp-sass
      // Adds auto-prefixer
      .pipe(sass({
        outputStyle: 'expanded'
      }))
      .pipe(prefix('last 4 versions'))
      .pipe(gulp.dest('./css/'))
  });

  gulp.task('watch', function() {
      gulp.watch('./assets/sass/**/*.scss',['sass']);
  });

  gulp.task('default',['sass']);


  Navigate to the sub_theme directory in terminal

    $ sites/all/themes/sub_theme

  Execute the intial test

    $ gulp hello

  Navigate to the sub_theme directory in terminal

    $ sites/all/themes/sub_theme

  Run the Sass task

    $ gulp sass

  Open style.scss in an IDE and write the following test:

    .testing {
      width: percentage(5/7);
      display: flex;
    }

Navigate to the sub_theme/css/style.css file in an IDE.

 The file should contain:

      .testing {
        width: 71.42857%;
        display: -webkit-flex;
        display: -ms-flexbox;
        display: flex;
      }

  From terminal, navigate to the sub_theme directory

    $ sites/all/themes/sub_theme

  Run the gulp watch task  
  (the style.css will be automatically updated when the scss file is changed)

    $ gulp watch

  Navigate back to the sub_theme/sass directory


  Navigate to the sub_theme/css/style.css

    The file should now be populated with bootstrap Sass files

  If not, run the gulp sass task in terminal

    $ gulp sass


####LINKING BOOTSTRAP JS TO SASS
  Navigate to the sub_theme directory

  Open the bootstrap_subtheme.libraries.yml file in an IDE

  Change the bootstrap-scripts to:

  bootstrap-scripts:
    js:
      bootstrap/javascripts/bootstrap/affix.js: {}
      bootstrap/javascripts/bootstrap/alert.js: {}
      bootstrap/javascripts/bootstrap/button.js: {}
      bootstrap/javascripts/bootstrap/carousel.js: {}
      bootstrap/javascripts/bootstrap/collapse.js: {}
      bootstrap/javascripts/bootstrap/dropdown.js: {}
      bootstrap/javascripts/bootstrap/modal.js: {}
      bootstrap/javascripts/bootstrap/tooltip.js: {}
      bootstrap/javascripts/bootstrap/popover.js: {}
      bootstrap/javascripts/bootstrap/scrollspy.js: {}
      bootstrap/javascripts/bootstrap/tab.js: {}
      bootstrap/javascripts/bootstrap/transition.js: {}

#### TURN OFF LOCAL CACHING

##### In the browser:
navigate to your site's /admin/config page
Under Development, click Performance
Under Caching, select <no caching>
Under Bandwidth Optimization, unclick Aggregate CSS files
Under Bandwidth Optimization, unclick Aggregate JavaScript files

##### In the project folder

* Copy and rename the sites/example.settings.local.php file as sites/default/settings.local.php
* Open settings.php file in sites/default directory and uncomment these lines:

  if (file_exists(__DIR__ . '/settings.local.php')) {
    include __DIR__ . '/settings.local.php';
  }

* Uncomment the following lines in settings.local.php to disable the render cache and dynamic page cache

  $settings['cache']['bins']['render'] = 'cache.backend.null';
  $settings['cache']['bins']['dynamic_page_cache'] = 'cache.backend.null';

* open development.services.yml in the sites folder and add the following lines (to disable twig cache)

  parameters:
    twig.config:
      debug : true
      auto_reload: true
      cache: false

* rebuild the Drupal cache

drush cache-rebuild | drush cr | admin/config/development/performance/ clear all caches

####Sources
  * [Chen Hui Jing](https://www.chenhuijing.com/blog/drupal-101-theming-with-gulp/)
  * [Drupal-Bootstrap-Subtheme](http://drupal-bootstrap.org/api/bootstrap/docs!subtheme!README.md/group/subtheme/7)
  * [Drupal-Bootstrap-Less](http://drupal-bootstrap.org/api/bootstrap/starterkits%21less%21README.md/group/subtheme_less/7)
  * [Create a Subtheme](http://www.zyxware.com/articles/5164/drupal-how-to-create-bootstrap-subtheme-in-drupal-7)
  * [Zell @ CSS Tricks](https://css-tricks.com/gulp-for-beginners/)

* Dan Hansen
* https://sevaa.com/blog/build-drupal-8-bootstrap-subtheme-sass/
* https://knackforge.com/blog/rajamohamed/disable-drupal-8-cache-during-development

####Requirements:
  Drupal 8

  NodeJS
