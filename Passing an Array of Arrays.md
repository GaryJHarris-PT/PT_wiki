1. Basic JavaScript Example

Here is how you define a function that accepts an array of arrays and loops through it:

```
function processMatrix($dataGrid) {
    for (var $i = 0; $i < $dataGrid.length; $i++) {
        var $row = $dataGrid[$i];
        var $itemNumber = $row;
        var $serialNumber = $row[4];
        
        _log("Processing Item: " + $itemNumber + " | Serial: " + $serialNumber);
    }
}

// Defining the 2D array
var $itemsToReceive = [
    ["PRNT444", "SN-1001"],
    ["PRTS-XY", "SN-1002"],
    ["46-00-00-LOCAL", "SN-1003"]
];

// Passing the array of arrays to the function
processMatrix($itemsToReceive);
```

---

2. Modern `for...of` & Destructuring Syntax

You can also use modern array destructuring directly inside the function parameters or loops:

```
function receiveBatchItems($batch) {
    for (var [$item, $serial] of $batch) {
        _log("Item: " + $item + " - Serial: " + $serial);
    }
}

// Call with an inline array of arrays
receiveBatchItems([
    ["PART-A", "SN01"],
    ["PART-B", "SN02"]
]);
```

---

3. Practical SAHI Pro Use Case

This is especially useful in SAHI test automation when you want a single library function to fill out dynamic forms or table rows in RepairData without writing repetitive code:

```
function fillInventoryGrid($lineItems) {
    for (var $i = 0; $i < $lineItems.length; $i++) {
        var $part = $lineItems[$i];
        var $qty  = $lineItems[$i][4];
        
        _setValue(_textbox("partNum_[" + $i + "]"), $part);
        _setValue(_textbox("quantity_[" + $i + "]"), $qty);
    }
}

// Calling the helper with a table of items and quantities
fillInventoryGrid([
    ["BEARING-01", "5"],
    ["FILTER-02", "12"],
    ["GASKET-03", "2"]
]);
```