## Уменьшение вложенности страниц

Изначально для данной задачи использовал [протокол Umap](http://dev.docs.umi-cms.ru/shablony_i_makrosy/xslt-shablonizator_umi_cms/formirovanie_dannyh_na_servere_protokol_umap/), но решение было не очень гибкое.

Для начала нужно перехватывать событие systemPrepare, которое вызывается уже после анализа URL и до генерации глобальных переменных.

Перехват события (файл events.php в папке classes/modules/catalog в директории шаблона, или custom_events.php в системной папке модуля):

```php
new umiEventListener('systemPrepare', 'catalog', 'short_links');
```

Права на доступ к макросу (permissions.php или permissions_custom.php):
```php
$permissions = array(
    'view' => array('short_links')
);
```

Сама функция с комментариями:
```php
// импортируем Service в самом начале файла
use UmiCms\Service;
//...
public function short_links(iUmiEventPoint $oEventPoint) {
    if ($oEventPoint->getMode() === "before") {
        // парсим url в массив
        $path_parts = Service::Request()->getPathParts();

        // у нас ссылки вида /product/{$alt-name}/
        // и соответственно здесь проверяется, соответствует ли ссылка данному шаблону
        @list($part1, $alt_name, $part3) = $path_parts;
        if ($part3 or $part1 !== 'product' or !$alt_name)
            return true;

        // ищем объект каталога по alt-name
        $pages = new selector('pages');
        $pages->types('hierarchy-type')->name('catalog', 'object');
        $pages->option('return')->value('id');
        $pages->where('alt_name')->equals($alt_name);
        $page_id = $pages->first();
        $page_id = $page_id ? @$page_id['id'] : false;
        if (!$page_id)
            return true;

        // переопределяем данные, чтобы сгенерировалась нужная страница
        $cmsController = cmsController::getInstance();
        $cmsController->setCurrentElementId($page_id);
        $cmsController->setCurrentModule('catalog');
        $cmsController->setCurrentMethod('object');
    }
    return true;
}
```

Для корректной работы нужно избавиться от товаров с одинаковым alt-name. Сделать это можно вот таким скриптом:

```php
define("CURRENT_WORKING_DIR", str_replace("\\", "/", $dirname = dirname(__FILE__)));
require CURRENT_WORKING_DIR . '/libs/root-src/standalone.php';

// здесь можно заменить catalog и object на данные необходимого иерархического типа данных, например news и item — для страниц новостей.
$type_id = umiHierarchyTypesCollection::getInstance()->getTypeByName('catalog', 'object')->getId();

$hierarchy = umiHierarchy::getInstance();
$connection = ConnectionPool::getInstance()->getConnection('core');

// прямым запросов к базе ищем дубли по alt-name
$result = $connection->query("SELECT GROUP_CONCAT(id SEPARATOR ', ') id, COUNT(*) c FROM `cms3_hierarchy` WHERE `type_id` = $type_id GROUP BY alt_name HAVING c > 1");

// распечатываем ссылки на страницы
while($row = mysqli_fetch_assoc($result)) {
	foreach (explode(',', $row['id']) as $id) {
        $page = $hierarchy->getElement($id);
		echo "<a href=\"/admin/catalog/edit/{$page->getId()}/\">{$page->getName()}</a><br />\n";
	}
	echo "\n";
}
```

Кладем скрипт в корень сайта и вызываем из браузера. Корректируем страницы до тех пор, пока дублей не останется.
