https://doc.qt.io/qt-6/qgraphicsscene.html

## Оглавление
- [Описание](#Описание)

## Описание
[Описание](https://doc.qt.io/qt-6/qgraphicsscene.html#details)

Класс служит контейнером для [`QGraphicsItems`](https://doc.qt.io/qt-6/qgraphicsitem.html). Он используется совместно с  [`QGraphicsView`](https://doc.qt.io/qt-6/qgraphicsview.html) для визуализации графических элементов, таких как линии, прямоугольники, текст или прочие нестандартные фигуры в 2D поверхности. `QGraphicsScene` является частью [`Graphics View Framework`](https://doc.qt.io/qt-6/graphicsview.html).

The class serves as a container for QGraphicsItems. It is used together with [QGraphicsView](https://doc.qt.io/qt-6/qgraphicsview.html) for visualizing graphical items, such as lines, rectangles, text, or even custom items, on a 2D surface. QGraphicsScene is part of the [Graphics View Framework](https://doc.qt.io/qt-6/graphicsview.html).

QGraphicsScene also provides functionality that lets you efficiently determine both the location of items, and for determining what items are visible within an arbitrary area on the scene. With the [QGraphicsView](https://doc.qt.io/qt-6/qgraphicsview.html) widget, you can either visualize the whole scene, or zoom in and view only parts of the scene.

Example:

[QGraphicsScene](https://doc.qt.io/qt-6/qgraphicsscene.html#QGraphicsScene) scene;
scene.addText("Hello, world!");

[QGraphicsView](https://doc.qt.io/qt-6/qgraphicsview.html) view(&scene);
view.show();

Note that QGraphicsScene has no visual appearance of its own; it only manages the items. You need to create a [QGraphicsView](https://doc.qt.io/qt-6/qgraphicsview.html) widget to visualize the scene.

To add items to a scene, you start off by constructing a QGraphicsScene object. Then, you have two options: either add your existing [QGraphicsItem](https://doc.qt.io/qt-6/qgraphicsitem.html) objects by calling [addItem](https://doc.qt.io/qt-6/qgraphicsscene.html#addItem)(), or you can call one of the convenience functions [addEllipse](https://doc.qt.io/qt-6/qgraphicsscene.html#addEllipse)(), [addLine](https://doc.qt.io/qt-6/qgraphicsscene.html#addLine)(), [addPath](https://doc.qt.io/qt-6/qgraphicsscene.html#addPath)(), [addPixmap](https://doc.qt.io/qt-6/qgraphicsscene.html#addPixmap)(), [addPolygon](https://doc.qt.io/qt-6/qgraphicsscene.html#addPolygon)(), [addRect](https://doc.qt.io/qt-6/qgraphicsscene.html#addRect)(), or [addText](https://doc.qt.io/qt-6/qgraphicsscene.html#addText)(), which all return a pointer to the newly added item. The dimensions of the items added with these functions are relative to the item's coordinate system, and the items position is initialized to (0, 0) in the scene.

You can then visualize the scene using [QGraphicsView](https://doc.qt.io/qt-6/qgraphicsview.html). When the scene changes, (e.g., when an item moves or is transformed) QGraphicsScene emits the [changed](https://doc.qt.io/qt-6/qgraphicsscene.html#changed)() signal. To remove an item, call [removeItem](https://doc.qt.io/qt-6/qgraphicsscene.html#removeItem)().

QGraphicsScene uses an indexing algorithm to manage the location of items efficiently. By default, a BSP (Binary Space Partitioning) tree is used; an algorithm suitable for large scenes where most items remain static (i.e., do not move around). You can choose to disable this index by calling [setItemIndexMethod](https://doc.qt.io/qt-6/qgraphicsscene.html#itemIndexMethod-prop)(). For more information about the available indexing algorithms, see the [itemIndexMethod](https://doc.qt.io/qt-6/qgraphicsscene.html#itemIndexMethod-prop) property.

The scene's bounding rect is set by calling [setSceneRect](https://doc.qt.io/qt-6/qgraphicsscene.html#sceneRect-prop)(). Items can be placed at any position on the scene, and the size of the scene is by default unlimited. The scene rect is used only for internal bookkeeping, maintaining the scene's item index. If the scene rect is unset, QGraphicsScene will use the bounding area of all items, as returned by [itemsBoundingRect](https://doc.qt.io/qt-6/qgraphicsscene.html#itemsBoundingRect)(), as the scene rect. However, [itemsBoundingRect](https://doc.qt.io/qt-6/qgraphicsscene.html#itemsBoundingRect)() is a relatively time consuming function, as it operates by collecting positional information for every item on the scene. Because of this, you should always set the scene rect when operating on large scenes.

One of QGraphicsScene's greatest strengths is its ability to efficiently determine the location of items. Even with millions of items on the scene, the [items](https://doc.qt.io/qt-6/qgraphicsscene.html#items)() functions can determine the location of an item within a few milliseconds. There are several overloads to [items](https://doc.qt.io/qt-6/qgraphicsscene.html#items)(): one that finds items at a certain position, one that finds items inside or intersecting with a polygon or a rectangle, and more. The list of returned items is sorted by stacking order, with the topmost item being the first item in the list. For convenience, there is also an [itemAt](https://doc.qt.io/qt-6/qgraphicsscene.html#itemAt)() function that returns the topmost item at a given position.

QGraphicsScene maintains selection information for the scene. To select items, call [setSelectionArea](https://doc.qt.io/qt-6/qgraphicsscene.html#setSelectionArea)(), and to clear the current selection, call [clearSelection](https://doc.qt.io/qt-6/qgraphicsscene.html#clearSelection)(). Call [selectedItems](https://doc.qt.io/qt-6/qgraphicsscene.html#selectedItems)() to get the list of all selected items.

### Event Handling and Propagation[](https://doc.qt.io/qt-6/qgraphicsscene.html#event-handling-and-propagation "Direct link to this headline")

Another responsibility that QGraphicsScene has, is to propagate events from [QGraphicsView](https://doc.qt.io/qt-6/qgraphicsview.html). To send an event to a scene, you construct an event that inherits [QEvent](https://doc.qt.io/qt-6/qevent.html), and then send it using, for example, [QCoreApplication::sendEvent](https://doc.qt.io/qt-6/qcoreapplication.html#sendEvent)(). [event](https://doc.qt.io/qt-6/qgraphicsscene.html#event)() is responsible for dispatching the event to the individual items. Some common events are handled by convenience event handlers. For example, key press events are handled by [keyPressEvent](https://doc.qt.io/qt-6/qgraphicsscene.html#keyPressEvent)(), and mouse press events are handled by [mousePressEvent](https://doc.qt.io/qt-6/qgraphicsscene.html#mousePressEvent)().

Key events are delivered to the _focus item_. To set the focus item, you can either call [setFocusItem](https://doc.qt.io/qt-6/qgraphicsscene.html#setFocusItem)(), passing an item that accepts focus, or the item itself can call [QGraphicsItem::setFocus](https://doc.qt.io/qt-6/qgraphicsitem.html#setFocus)(). Call [focusItem](https://doc.qt.io/qt-6/qgraphicsscene.html#focusItem)() to get the current focus item. For compatibility with widgets, the scene also maintains its own focus information. By default, the scene does not have focus, and all key events are discarded. If [setFocus](https://doc.qt.io/qt-6/qgraphicsscene.html#setFocus)() is called, or if an item on the scene gains focus, the scene automatically gains focus. If the scene has focus, [hasFocus](https://doc.qt.io/qt-6/qgraphicsscene.html#hasFocus)() will return true, and key events will be forwarded to the focus item, if any. If the scene loses focus, (i.e., someone calls [clearFocus](https://doc.qt.io/qt-6/qgraphicsscene.html#clearFocus)()) while an item has focus, the scene will maintain its item focus information, and once the scene regains focus, it will make sure the last focus item regains focus.

For mouse-over effects, QGraphicsScene dispatches _hover events_. If an item accepts hover events (see [QGraphicsItem::acceptHoverEvents](https://doc.qt.io/qt-6/qgraphicsitem.html#acceptHoverEvents)()), it will receive a [GraphicsSceneHoverEnter](https://doc.qt.io/qt-6/qevent.html#Type-enum) event when the mouse enters its area. As the mouse continues moving inside the item's area, QGraphicsScene will send it [GraphicsSceneHoverMove](https://doc.qt.io/qt-6/qevent.html#Type-enum) events. When the mouse leaves the item's area, the item will receive a [GraphicsSceneHoverLeave](https://doc.qt.io/qt-6/qevent.html#Type-enum) event.

All mouse events are delivered to the current _mouse grabber_ item. An item becomes the scene's mouse grabber if it accepts mouse events (see [QGraphicsItem::acceptedMouseButtons](https://doc.qt.io/qt-6/qgraphicsitem.html#acceptedMouseButtons)()) and it receives a mouse press. It stays the mouse grabber until it receives a mouse release when no other mouse buttons are pressed. You can call [mouseGrabberItem](https://doc.qt.io/qt-6/qgraphicsscene.html#mouseGrabberItem)() to determine what item is currently grabbing the mouse.

**See also** [QGraphicsItem](https://doc.qt.io/qt-6/qgraphicsitem.html) and [QGraphicsView](https://doc.qt.io/qt-6/qgraphicsview.html).