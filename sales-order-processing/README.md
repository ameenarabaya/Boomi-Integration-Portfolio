# Sales Order Processing & Discount Logic

## Description
This Boomi integration processes sales order data and applies business
rules to calculate discounts based on the total order amount.

## Integration Flow
- Receives sales order data
- Converts data between JSON and XML profiles
- Calculates the total order amount
- Applies discount logic when the total exceeds a defined threshold
- Sets process properties based on discount rules
- Sends email notifications when discount conditions are met
- Ends the process after successful execution

## Business Rules
- If total order amount is greater than a specific value, a discount is applied
- Discount percentage is calculated dynamically
- Notifications are triggered for eligible orders

## Key Concepts Used
- Decision Shapes
- Data Mapping
- Process Properties
- Email Notification
- Conditional Logic

## Tools & Technologies
- Boomi AtomSphere
- JSON / XML Profiles
- Mail Connector

### Sample Script
```javascript
load("nashorn:mozilla_compat.js");
importClass(com.boomi.execution.ExecutionUtil);

for (var i = 0; i < dataContext.getDataCount(); i++) {
    var is = dataContext.getStream(i);
    var props = dataContext.getProperties(i);

    var scanner = new java.util.Scanner(is, "UTF-8").useDelimiter("\\A");
    var xmlString = scanner.hasNext() ? scanner.next() : "";
    scanner.close();

    var factory = javax.xml.parsers.DocumentBuilderFactory.newInstance();
    var builder = factory.newDocumentBuilder();
    var doc = builder.parse(new java.io.ByteArrayInputStream(xmlString.getBytes("UTF-8")));

    var items = doc.getElementsByTagName('item');
    var total = 0;

    for (var j = 0; j < items.length; j++) {
        var currentItem = items.item(j);
        var quantity = parseFloat(currentItem.getElementsByTagName('quantity').item(0).textContent);
        var price = parseFloat(currentItem.getElementsByTagName('price_per_unit').item(0).textContent);
        total += quantity * price;
    }

    var rootElement = doc.getDocumentElement();
    var totalElement = doc.createElement('total_amount');
    totalElement.textContent = total.toFixed(2);
    rootElement.appendChild(totalElement);

    props.setProperty("document.dynamic.userdefined.total_amount", total.toFixed(2));

    var transformer = javax.xml.transform.TransformerFactory.newInstance().newTransformer();
    var writer = new java.io.StringWriter();
    transformer.transform(
        new javax.xml.transform.dom.DOMSource(doc),
        new javax.xml.transform.stream.StreamResult(writer)
    );

    var newIs = new java.io.ByteArrayInputStream(writer.toString().getBytes("UTF-8"));
    dataContext.storeStream(newIs, props);
}

## Script-Based Discount Calculation

This script calculates the discount and final price based on a dynamic
discount value stored in Boomi document properties.

### Script Logic Overview
- Reads the current total amount from the XML document
- Retrieves the discount value from a dynamic document property
- Calculates the total discount and final price
- Stores calculated values as dynamic document properties
- Outputs the updated document for further processing

### Sample Script
```javascript
for (var i = 0; i < dataContext.getDataCount(); i++) {
    var is = dataContext.getStream(i);
    var props = dataContext.getProperties(i);

    var scanner = new java.util.Scanner(is, "UTF-8").useDelimiter("\\A");
    var xmlString = scanner.hasNext() ? scanner.next() : "";
    scanner.close();

    var factory = javax.xml.parsers.DocumentBuilderFactory.newInstance();
    var builder = factory.newDocumentBuilder();
    var doc = builder.parse(new java.io.ByteArrayInputStream(xmlString.getBytes("UTF-8")));

    var totalAmountElement = doc.getElementsByTagName("total_amount").item(0);
    var currentTotal = parseFloat(totalAmountElement.getTextContent());

    var discountProperty = props.getProperty("document.dynamic.userdefined.discount");
    if (discountProperty == null) {
        throw "Discount property 'document.dynamic.userdefined.discount' not found";
    }

    var discountRate = parseFloat(discountProperty);
    var total_discount = currentTotal * discountRate;
    var final_price = currentTotal - total_discount;

    props.setProperty("document.dynamic.userdefined.total_discount", total_discount.toFixed(2));
    props.setProperty("document.dynamic.userdefined.final_price", final_price.toFixed(2));

    var transformer = javax.xml.transform.TransformerFactory.newInstance().newTransformer();
    var writer = new java.io.StringWriter();
    transformer.transform(
        new javax.xml.transform.dom.DOMSource(doc),
        new javax.xml.transform.stream.StreamResult(writer)
    );

    var newIs = new java.io.ByteArrayInputStream(writer.toString().getBytes("UTF-8"));
    dataContext.storeStream(newIs, props);
}



