## Управление обновлением карты сайта

Может понадобиться управлять механизмом обновления некоторых страниц карты сайта.

Не так давно появилась новая стандартная точка вызова before_update_sitemap, которой нет в документации.

Механизм назначения обработчика подробно описан в разделе [Событийная модель UMI.CMS](http://api.docs.umi-cms.ru/razrabotka_nestandartnogo_funkcionala/sobytijnaya_model_umicms)

 Точки вызова before_update_sitemap:
  Вызывается при подготовке глобальных переменных
   режим: before
   параметры: 
   
    (int) id - id страницы
    
   параметры-ссылки: 
   
    (bool) &robots_deny - запрет для отображения в карте сайта
    (string) &link - ссылка страницы

Пример обработчика:
```php
 public function update_sitemap(iUmiEventPoint $oEventPoint) {
     if ($oEventPoint->getMode() === "before") {
         $link = &$oEventPoint->getRef('link');
         $robots_deny = &$oEventPoint->getRef('robots_deny');
         @$page_id = &$oEventPoint->getParam('id');
         $page = $page_id ? umiHierarchy::getInstance()->getElement($page_id) : false;
         if (!$page instanceof umiHierarchyElement) return true;
         // исключаем из карты сайта все комментарии
         if ($page->getModule() == 'comments') $robots_deny = true;

         // кастомный метод для определения, для каких страниц нужно отдавать 404-страницу
         if ($this->isStatus404($page_id)) $robots_deny = true;

         // на сайте используются модифицированные адреса объектов каталога
         if ($page->getMethod() == 'object') {
             $link = '{your_domain}' . $page->getAltName() . '/';
         }
     }
     return true;
 }
```