## Удаление изображений при удалении страниц
Назначаем обработчик события:

```php
new umiEventListener('systemDeleteElement', 'catalog', 'onRemoveItem');
```

Задаем права доступа к макросу:

```php
$permissions = array('view' => array('onRemoveItem'));
```

Непосредственно обработчик:

```php
/**Удаление изображений при удалении страницы
 * @param iUmiEventPoint $oEventPoint
 * @return bool
 */
public function onRemoveItem(iUmiEventPoint $oEventPoint) {
    if ($oEventPoint->getMode() === "after") {
        $element = $oEventPoint->getRef("element");
        $photo = @ $element->getValue('photo');
        if ($photo instanceof umiImageFile)
            $photo->delete();
    }
    return true;
}
```