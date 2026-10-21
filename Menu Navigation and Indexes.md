
The recommended way to navigate submenus without relying on positional indexes (, ) in SAHI Pro is using **Relational Accessors** (specifically **_under** or **_in**).

In ASP.NET menu bars where terms like "Find" or "Item Number" exist under multiple header tabs (such as Asset, Inventory, and Work Order), SAHI uses positional indexes by default because it sees multiple matching links in the DOM tree. By anchoring the click target relative to the visual parent header tab, SAHI evaluates only the links inside that specific menu dropdown.

Here is how to anchor the menu links relationally:

_click(_link("Inventory"));

_click(_link("Find", _under(_link("Inventory"))));

_click(_link($lbl_ItemNumber, _under(_link("Find"))));

If "Item number" is a child item inside the Inventory menu container element, you can also anchor it directly to the parent node of the top link:

_click(_link("Inventory"));

_click(_link($lbl_ItemNumber, _in(_parentNode(_link("Inventory")))));

Anchoring with _under(_link("Inventory")) restricts SAHI's element search to the visual area beneath the "Inventory" tab, ignoring the "Find" links under Asset or Work Order and removing the need for index numbers.