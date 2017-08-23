# ZFF-I NOTES

NOTE TO SELF: find better example of delegator

http://localhost:9090/#/5/45

## ERRATA
* http://localhost:9090/#/2/5: s/be ´event´
* http://localhost:9090/#/3/22: s/be onlinemarket.work
* http://localhost:9090/#/3/19: there is no folder "Model"
* http://localhost:9090/#/4/10: s/be `<module_name>/src/Module.php` as per convention in composer.json.  Here is an example:
```
"autoload": {
    "psr-4": {
        "Application\\": "module/Application/src/",
        "Guestbook\\": "module/Guestbook/src/",
        "Login\\": "module/Login/src/",
        "Events\\": "module/Events/src/"
    }
},
```
* http://localhost:9090/#/4/27: s/be ´SOME VALUE´
* http://localhost:9090/#/4/44: s/be "escape" not "excape"
* http://localhost:9090/#/5/10: need to add `CreateHttpNotFoundModel` to list of plugins
* http://localhost:9090/#/5/11: delete this slide: AcceptableViewModelSelector is covered in ZFF-II
* http://localhost:9090/#/5/33: already discussed this!
* http://localhost:9090/#/5/41: drop template_map == too confusing
* http://localhost:9090/#/5/22: suggest moving discussion of `url()` plugin before discussion of `redirect()` plugin as concepts are similar, but `url()` plugin is easier to understand
* http://localhost:9090/#/6/16 - 23: initializers, abstract factories and delegators are covered extensively in the ZFF-II


## LABS
### Important Note

You need to disable Zend Server OpCache and PageCache ... otherwise changes you make might not be recognized right away as the opcode or output has been cached
* From browser: `http://localhost:10081`
* Administration
* Components
* Checkmark `Zend OPcache` and `Zend Page Cache`
* Click on the `Disable` button at the top
* Restart Zend Server

### First Lab:
* Need to add onlinemarket.work apache vhost link
* Do not do steps 7 and 8
* Need to add a step assigning rights to `www-data`
* Need to add entry for onlinemarket.work to `/etc/hosts`
  
### Module Lab:
* make sure you update security rights for new module 
```
cd /home/vagrant/Zend/workspaces/DefaultWorkspace/onlinemarket.work
sudo chown -R module/Market
```
* path for controller s/be: `onlinemarket.work\module\Market\src\Controller\IndexController.php`
* s/be `Market\config\module.config.php`

* If you get this message:
```
Fatal error: Uncaught Zend\ModuleManager\Exception\RuntimeException: Module (NAME) could not be initialized
```
* Make sure you do the following:
	* there MUST be a file `/modules/NAME/src/Module.php` with a class `Module` using the module namespace
	* add the module to the array in /config/modules.config.php
	* add an entry in composer.json for autoloading
	* run `composer update` or `composer dump-autoload`

### Plugins Lab
* Directory s/be: `Market\src\Controller\Plugin` not `plugin`

### New Controllers and Factories Lab
* Steps 8 + 17: I would drop the `template_map` in favor of `template_path_stack`
* Steps 9 + 18: too early to discuss child_routes: I would recommend just single level routes at this stage

## Q & A
* Q: Do you need git installed to use composer?
* A: If the repository from which Composer draws is based on github.com or bitbucket.org you do not have to install git.  Otherwise, if the packages uses something else (e.g. svn), you need to have that client installed.

* Q: Is there a list of Module::get*Config() methods?
* A: Yes: look here https://docs.zendframework.com/zend-modulemanager/module-manager/ about the middle of the page

* Q: Is there a tool to generate a template map?
* A: /path/to/vendor/bin/templatemap_generator.php /path/to/module/MODULE_NAME

## Wed 16 Aug 2017

### Example of setting alternate template:
```
<?php
/**
 * @link      http://github.com/zendframework/ZendSkeletonApplication for the canonical source repository
 * @copyright Copyright (c) 2005-2016 Zend Technologies USA Inc. (http://www.zend.com)
 * @license   http://framework.zend.com/license/new-bsd New BSD License
 */

namespace Application\Controller;

use Zend\Mvc\Controller\AbstractActionController;
use Zend\View\Model\ViewModel;

class IndexController extends AbstractActionController
{
    public function indexAction()
    {
        return new ViewModel();
    }
    public function testAction()
    {
        $viewModel = new ViewModel(['controller' => __CLASS__]);
        $viewModel->setTemplate('application/index/index');
        return $viewModel;
    }
}
```

