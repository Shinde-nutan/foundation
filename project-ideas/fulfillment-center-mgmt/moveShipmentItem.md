## **moveShipmentItem**.

This is API method is part of the order fulfillment applications facade. 

**Purpose**

The primary goal of the `moveShipmentItem` service is to transfer a specific item from one shipment to another within the same order. This is useful in scenarios where items need to be rearranged or consolidated during the fulfillment process.

**Workflow**
*  Shipment can be edited in INPUT status. Check if shipment is in input status, if not move it to input status before editing its content. 

*  Moving shipment item is three step process 
  1. Delete shipmentItem from existing shipment and then add it to other shipment. 
  2. In some cases the shipmentItem is moved to new shipment, in such case we may have to create shipment. 
  3. Add shipmentItem to the shipment. 


**Input Parameters**

*   **`shipmentId` (String):** The ID of the original shipment from which the item is being moved.
*   **`shipmentPackageSeqId` (String, optional):** The sequence ID of the package within the original shipment that contains the item.
*   **`toShipmentId` (String):** The ID of the destination shipment to which the item will be moved.
*   **`toShipmentPackageSeqId` (String, optional):** The sequence ID of the package within the destination shipment where the item should be placed.
*   **`shipmentItemSeqId` (String):** The sequence ID of the specific item within the shipment that is being moved.
*   **`quantity` (BigDecimal, optional):** The quantity of the item to move. If not provided, the entire quantity of the shipment item is moved.
*   **`userLogin` (GenericValue):** The userLogin object representing the user performing the action.

**Use Cases**

This service is typically used in the following scenarios:

*   **Consolidating Shipments:** If multiple shipments are going to the same address, it might be more efficient to combine items into a single shipment.
*   **Correcting Errors:** If an item was accidentally placed in the wrong shipment, this service can be used to move it to the correct one.
*   **Splitting Shipments:** If a shipment becomes too large or heavy, some items can be moved to a new shipment.

**Workflow**

1.  **Input Validation:** The service validates the input parameters to ensure they are correct and consistent. It checks for the existence of the shipments, verifies that the shipments belong to the same order, and confirms the validity of the shipment item and quantity.

2.  **Remove Item from Original Shipment:** The service calls the `deleteOrderShipmentItem` service to remove the item from the original shipment.

3.  **Add Item to New Shipment:** The service calls the `createOrderShipmentItem` service to add the the item to the new shipment. It uses the `OrderShipment` entity to associate the item with the correct order and shipment.

