## Ajax-подгрузка новостей
```php
public function getGallery($parentId = 0, $limit = false, $is_ajax = 0) {
    $module = $this->module ? $this->module : $this;
    $limit = ($limit) ? $limit : $module->per_page;
    $currentPage = (int) getRequest('p');
    $offset = $currentPage * $limit;

    $pages = new selector('pages');
    $pages->types('hierarchy-type')->name('news', 'item');
    $pages->where('hierarchy')->page($parentId)->level(2);
    $pages->order('ord');
    $pages->limit($offset, $limit);

    $result = array();
    $total = $pages->length();
    $result['nodes:page'] = array();
    foreach($pages->result as $page) {
        $result['nodes:page'][] = $page;
    }
    $result['total'] = $total;
    $result['per_page'] = $limit;
    $result['stop'] = (int) (($offset + count($result['nodes:page'])) == $total);
    $result['category'] = $parentId;

    // кусок, отвечающий за рендер
    if ($is_ajax) {
        $currentTemplater = cmsController::getInstance()->getCurrentTemplater();
        $templatesSource = $currentTemplater->getTemplatesSource();
        $templater = new umiTemplaterPHP($templatesSource);
        $buffer = OutputBuffer::current('HTTPOutputBuffer');
        $buffer->push($templater->render($result, 'news/inc/gallery'));
        $buffer->end();
    }

    return $result;
}
```
В данном примере используется шаблон news/inc/gallery.
Лучше не передавать его снаружи из соображений безопасности, либо делать это с большой осторожностью.

Пример вызова: http://localhost/news/getGallery/{page_id}/{page_number}/1/

Если $is_ajax=true, то рендерится html.
