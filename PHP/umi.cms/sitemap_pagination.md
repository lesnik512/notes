## Постраничная карта сайта

Ниже приводится код основного макроса и двух вспомогательных.

```php
class content_custom extends def_module {
    public $sitemap_count_down;
    public $sitemap_offset;
    public $sitemap_iterator = 0;
    
    /**Постраничный вывод карты сайта
     * @param int $limit количество ссылок на странице
     * @param int $expire время жизни кеша в секундах
     * @return array
     * @throws selectorException
     */
    public function sitemap_pages($limit=500, $expire=7200) {
        $page = (int) getRequest('p');

        $block_arr = array();
        $this->sitemap_count_down = $limit;
        $this->sitemap_offset = $page * $this->sitemap_count_down;

        $children_count = $this->sitemap_children_count(0);

        if ($this->sitemap_offset > $children_count)
            return $block_arr['items'] = 'none';

        // кеширование данных карты сайта в файлы
        $folder = CURRENT_WORKING_DIR . '/sys-temp/sitemap-cache/';
        if(!is_dir($folder))
            mkdir($folder, 0777, true);
        $data_path = $folder . 'map' . $page . '.html';
        $mtime = is_file($data_path) ? filemtime($data_path) : false;
        if($mtime && (time() < ($mtime + $expire)))
            $block_arr['items']['nodes:item'] = json_decode(file_get_contents($data_path),true);
        else {
            $block_arr['items']['nodes:item'] = $this->sitemap_children(0);
            file_put_contents($data_path, json_encode($block_arr['items']['nodes:item']));
        }

        $block_arr['total'] = $children_count;
        $block_arr['per_page'] = 500;
        return $block_arr;
    }

    /**Рекурсивная функция для генерации данных карты сайта
     * @param int $page_id id страницы
     * @return array
     * @throws selectorException
     */
    public function sitemap_children($page_id) {
        $hierarchy = umiHierarchy::getInstance();
        $pages = new selector('pages');
        $pages->where('hierarchy')->page($page_id)->level(1);
        $pages->where('robots_deny')->notequals(1);
        $pages->where('is_active')->equals(1);
        $pages->where('is_visible')->equals(1);
        $pages->order('ord');

        $items = array();

        foreach ($pages as $page) {
            /** @var umiHierarchyElement $page */
            if (!isset($this->sitemap_iterator))
                $this->sitemap_iterator = 0;
            if (!$this->sitemap_count_down)
                break;
            $children_count = $this->sitemap_children_count($page->getId()) + 1;
            if ($this->sitemap_offset >= $children_count) {
                $this->sitemap_offset -= $children_count;
                $this->sitemap_iterator += $children_count;
            } else {
                $this->sitemap_iterator++;
                if ($this->sitemap_offset > 0) {
                    $this->sitemap_offset--;
                    $item_arr = array();
                }
                else {
                    $this->sitemap_count_down--;
                    $item_arr = array(
                        '@i' => $this->sitemap_iterator,
                        '@link' => $page->link,
                        '@name' => $page->getName(),
                    );
                }
                if (($children_count>1) && $this->sitemap_count_down)
                    $item_arr['items']['nodes:item'] = $this->sitemap_children($page->getId());
                $items[] = $item_arr;
            }
            $hierarchy->unloadElement($page->getId());
        }
        return $items;
    }

    /**Количество активных и видимых подстраниц на неограниченную вложенность
     * @param int $page_id id страницы
     * @return bool|int
     */
    public function sitemap_children_count($page_id) {
        $hierarchy = umiHierarchy::getInstance();
        return $hierarchy->getChildrenCount($page_id, false, false);
    }
}
```

Права доступа к методам класса:

```php
$permissions = Array(
    'content' => array('sitemap_pages', 'sitemap_children', 'sitemap_children_count')
);
```

Пример шаблона на php-шаблонизаторе:

```php
$p = getRequest('p');
$sitemap = $this->macros('content', 'sitemap_pages', array());
function renderMap($map) {
    $result = '';
    if(isset($map['items']['nodes:item'])) {
        $result .= '<ul>';
        foreach($map['items']['nodes:item'] as $item) {
            $result .= '<li>';
            if (isset($item['@link'])) $result .= "<a href=\"{$item['@link']}\">{$item['@name']}</a>";
            $result .= renderMap($item);
            $result .= '</li>';
        }
        $result .= '</ul>';
    }
    return $result;
}
?>
<div class="b-sitemap">
    <?=renderMap($sitemap)?>
</div>
<?=@($sitemap['total'] > $sitemap['per_page']) ? $this->render($this->macros('system','numpages', array($sitemap['total'], $sitemap['per_page'])), 'library/numpages') : ''
```