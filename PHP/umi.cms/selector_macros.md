## Кастомный макрос с выборкой

```php
/**Постраничный вывод объектов каталога 
 * @param int $parentId id раздела для поиска страниц
 * @param bool $limit количество объектов на странице
 * @param array $fields массив с дополнительными полями, которые нужны
 * @return array
 * @throws selectorException
 */
public function getObjects($parentId = 0, $limit = false, $fields = array()) {
    $limit = ($limit) ? $limit : $this->module->per_page;
    $currentPage = (int) getRequest('p');
    $offset = $currentPage * $limit;
    if (!is_array($fields)) $fields = explode(',',$fields);

    // объединяем общий список полей с переданным через параметр $fields
    $fields = array_merge(array('id', 'name', 'alt_name'), $fields);

    $hierarchy = umiHierarchy::getInstance();
    $pages = new selector('pages');
    $pages->types('hierarchy-type')->name('catalog', 'object');
    $pages->where('hierarchy')->page($parentId)->level(2);

    // selector вернет вместо объектов umiHierarchyElement ассоциативный массив
    $pages->option('return')->value($fields);
    $pages->order('ord')->asc();
    $pages->limit($offset, $limit);

    $result = array();
    $total = $pages->length();
    foreach($pages->result as $page) {
        $item = $page;
        $item['link'] = $hierarchy->getPathById($item['id']);
        $result['nodes:page'][] = $item;
    }
    $result['total'] = $total;
    $result['per_page'] = $limit;
    return $result;
}
```

Метод работает в php- и xslt-шаблонизаторах.