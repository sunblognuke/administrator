## Intro

* 中文讨论请前往：https://phphub.org/topics/2301
* 中文教程见这里：https://phphub.org/topics/2407


Forked from [FrozenNode/Laravel-Administrator](https://github.com/FrozenNode/Laravel-Administrator) with the following changes:

* UI Improved
* UX Improved (Editor view stick, hover effect etc.)
* Model deletion with Sweet alert confirmation
* Batch model deletion
* Refresh btn
* Reduce page css and js file request number
* Edit view hint

 ~~only intent to support Laravel 5.1.*~~

![1](https://cloud.githubusercontent.com/assets/324764/16544619/6db648d0-413f-11e6-8842-bf0b993416ef.png)

![2](https://cloud.githubusercontent.com/assets/324764/16544623/72a8c0ac-413f-11e6-9c5b-0259b07a7c37.png)

## Install

### 1. composer require

```
composer require "summerblue/administrator:^1.0"
```

### 2. add provider

Edit `config/app.php` in `providers` array add provider:

```php
'providers' => [
	Frozennode\Administrator\AdministratorServiceProvider::class,
]
```

### 3. publish assets/config

config/administrator.php:

```
php artisan vendor:publish
```

### 4. 需要注意的地方和代码示例：

```
<?php

return array(

    /*
     * Package URI
     *
     * @type string
     */
    'uri' => 'admin',

    /*
     *  Domain for routing.
     *
     *  @type string
     */
    'domain' => '',

    /*
     * Page title
     *
     * @type string
     */
    'title' => '后台管理::Dashboard',

    /*
     * The path to your model config directory
     *
     * @type string
     */
    'model_config_path' => config_path('administrator'),

    /*
     * The path to your settings config directory
     *
     * @type string
     */
    'settings_config_path' => config_path('administrator/settings'),

    /*
     * The menu structure of the site. For models, you should either supply the name of a model config file or an array of names of model config
     * files. The same applies to settings config files, except you must prepend 'settings.' to the settings config file name. You can also add
     * custom pages by prepending a view path with 'page.'. By providing an array of names, you can group certain models or settings pages
     * together. Each name needs to either have a config file in your model config path, settings config path with the same name, or a path to a
     * fully-qualified Laravel view. So 'users' would require a 'users.php' file in your model config path, 'settings.site' would require a
     * 'site.php' file in your settings config path, and 'page.foo.test' would require a 'test.php' or 'test.blade.php' file in a 'foo' directory
     * inside your view directory.
     *
     * @type array
     *
     * 	array(
     *		'E-Commerce' => array('collections', 'products', 'product_images', 'orders'),
     *		'homepage_sliders',
     *		'users',
     *		'roles',
     *		'colors',
     *		'Settings' => array('settings.site', 'settings.ecommerce', 'settings.social'),
     * 		'Analytics' => array('E-Commerce' => 'page.ecommerce.analytics'),
     *	)
     */
    'menu' => [
//        '用户管理' => [
//            'users' // 对应需要创建的文件：`config/administrator/users.php`
//        ],
        '内容管理' => [
            'tasks' // 对应需要创建的文件：`config/administrator/tasks.php`
        ]
    ],

    /*
     * The permission option is the highest-level authentication check that lets you define a closure that should return true if the current user
     * is allowed to view the admin section. Any "falsey" response will send the user back to the 'login_path' defined below.
     *
     * @type closure
     */
    'permission' => function () {
        //return Auth::check();
        return true;
    },

    /*
     * This determines if you will have a dashboard (whose view you provide in the dashboard_view option) or a non-dashboard home
     * page (whose menu item you provide in the home_page option)
     *
     * @type bool
     */
    'use_dashboard' => false,

    /*
     * If you want to create a dashboard view, provide the view string here.
     *
     * @type string
     */
    'dashboard_view' => '',

    /*
     * The menu item that should be used as the default landing page of the administrative section
     *
     * @type string
     */
    'home_page' => 'tasks',

    /*
     * The route to which the user will be taken when they click the "back to site" button
     *
     * @type string
     */
    'back_to_site_path' => '/',

    /*
     * The login path is the path where Administrator will send the user if they fail a permission check
     *
     * @type string
     */
    'login_path' => 'auth/login',

    /*
     * The logout path is the path where Administrator will send the user when they click the logout link
     *
     * @type string
     */
    'logout_path' => false,

    /*
     * This is the key of the return path that is sent with the redirection to your login_action. Session::get('redirect') will hold the return URL.
     *
     * @type string
     */
    'login_redirect_key' => 'redirect',

    /*
     * Global default rows per page
     *
     * @type int
     */
    'global_rows_per_page' => 20,

    /*
     * An array of available locale strings. This determines which locales are available in the languages menu at the top right of the Administrator
     * interface.
     *
     * @type array
     */
    'locales' => array(),

    'custom_routes_file' => app_path('Http/routes/administrator.php'),
);

```

config/administrator/tasks.php

```
<?php

use App\Task;

return [

    'title' => '任务管理',
    'description' => '任务管理',
    'single' => '任务',
    'heading' => '任务管理', 
    'model' => Task::class,

    'columns' => [
        'id' => [
            'title' => 'ID'
        ],
        'name' => [
            'title' => '标题',
        ],
        'description' => [
            'title' => '简介',
            'sortable' => false,
            'output' => function($value)
            {
                return str_limit($value);
            },
        ],
//        'user_name' => [
//            'title' => "Author",
//            'relationship' => 'user', //this is the name of the Eloquent relationship method!
//            'select' => "(:table).name",
//        ],
//        'created_at',

        'operation' => [
            'title'  => '管理',
            'output' => function ($value, $model) {
                return $value;
            },
            'sortable' => false,
        ],
    ],

    'edit_fields' => [
        'name' => [
            'title' => '标题',
            'type' => 'text'
        ],
        'description' => [
            'title' => '简介',
            'type' => 'textarea'
        ],
//        'user' => array(
//            'type' => 'relationship',
//            'title' => 'Author',
//            'name_field' => 'name',
//        )
    ],

    'filters' => [
        'name' => [
            'title' => '标题',
        ]
    ],

];
```

必填项：

'title' => '任务管理',
'description' => '任务管理',
'single' => '任务',
'heading' => '任务管理', 
'model' => Task::class,

Read the docs: [FrozenNode/Laravel-Administrator](https://github.com/FrozenNode/Laravel-Administrator)

-- end
