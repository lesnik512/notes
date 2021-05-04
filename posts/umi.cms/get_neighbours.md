## Макрос для получения «соседей»

Из коробки есть макросы %system getNext()% и %system getPrevious()%.

Привожу пример объединенного сокращенного макроса. Принимает только один параметр: id страницы или путь.

Использую для сокращения количества запросов.

```php
public function get_neighbours($path = '') {
    $element_id = def_module::analyzeRequiredPath($path);
    $element = $element_id ? umiHierarchy::getInstance()->getElement($element_id) : false;

    if(!$element)
        return '';

    $pages = new selector('pages');
    $pages->types('hierarchy-type')->id($element->getTypeId());
    $pages->where('hierarchy')->page($element->getParentId())->level(1);
    $pages->order('ord');
    $pages->option('return')->value('id');

    $next = false;
    $prev = false;
    $is_matched = false;

    foreach($pages->result() as $page) {
        $id = $page['id'];
        if($is_matched) {
            $next = $id;
            break;
        }
        if($id == $element_id)
            $is_matched = true;
        if (!$is_matched)
            $prev = $id;
    }

    return [
        'prev_id' => $prev,
        'prev_link' => umiHierarchy::getInstance()->getPathById($prev),
        'next_id' => $next,
        'next_link' => umiHierarchy::getInstance()->getPathById($next)
    ];
}
```

Пример запроса:

```
https://somesite.ru/udata:/news/get_neighbours/456
```

Пример xml-ответа:

```xml
<udata xmlns:xlink="http://www.w3.org/TR/xlink" module="news" method="get_neighbours" generation-time="0.035878">
    <prev_id>454</prev_id>
    <prev_link>/news/lollipop-popular-android/</prev_link>
    <next_id>457</next_id>
    <next_link>/news/microsoft-surface-3-i-surface-pro-4-cases/</next_link>
</udata>
```