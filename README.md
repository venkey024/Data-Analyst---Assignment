# Courier Charges Verification

## Project Overview

**Client**: A large e-commerce company in India (Company X).

**Objective**: Verify if the shipping charges billed by courier companies match Company X's internal calculations to ensure accurate billing and cost management.

---

## Problem Statement

Company X processes thousands of orders daily, resulting in significant shipping costs. To avoid discrepancies and ensure accurate billing, it is crucial to compare the charges billed by courier companies with those calculated internally using Company X's rate cards.

---

## Data Sources

### Input Data (Left Hand Side - LHS)

1. **Website Order Report**:
   - **Order IDs** and associated SKUs.
   - **Order ID**: Common identifier between Company X’s records and courier invoices.

2. **SKU Master**:
   - **Gross Weight**: Weight of each product in grams.

3. **Warehouse Pincode Mapping**:
   - Maps warehouse and customer pincodes to delivery zones (a/b/c/d/e).

### RHS Data (Courier Company Invoice in CSV File)

1. **Invoice Details**:
   - **AWB Number**: Courier’s internal shipment ID.
   - **Order ID**: Company X’s order ID.
   - **Weight of Shipment**: Weight reported by the courier company.
   - **Warehouse Pickup Pincode**: Pincode where the shipment is picked up.
   - **Customer Delivery Pincode**: Pincode where the shipment is delivered.
   - **Zone of Delivery**: Delivery zone assigned by the courier (a/b/c/d/e).
   - **Charges per Shipment**: Total charges billed.
   - **Type of Shipment**: Indicates if the charges are “Forward charges” or “Forward and RTO charges”.

2. **Courier Charges Rate Card**:
   - **Forward Charges**: Charges for shipping only.
   - **Forward and RTO Charges**: Charges for shipping and return.
   - **Charge Calculation**:
     - Fixed rate for the first 0.5 KG.
     - Additional charges for every 0.5 KG beyond the first slab.
     - **Total Charges** = Fixed rate + Additional charges.

---

## Steps for Analysis

### Step 1: Importing Data

1. **Order Report**:
   - `ExternOrderNo`: Unique order numbers.
   - `SKU`: Unique product identifier.
   - `Order Qty`: Number of units ordered.

2. **Pincode Zones**:
   - `Warehouse Pincode`: Company X’s warehouse pincode.
   - `Customer Pincode`: Delivery pincode.
   - `Zone`: Delivery zone (a/b/c/d/e).

3. **SKU Master**:
   - `SKU`: Unique product identifier.
   - `Weight (g)`: Weight of the product in grams.

### Step 2: Data Manipulation

- **Weight Calculation**:
  - Calculate total weight per order.
  - Determine weight slab based on the total weight.
  - Add a new column `slab_count_courier` to reflect the number of 0.5 KG increments.

### Step 3: Charge Calculation

- **Calculation Details**:
  - Use rate cards to compute expected charges based on weight slabs and delivery zones.
  - For “Forward charges”, apply only the forward charge rate.
  - For “Forward and RTO charges”, apply both forward and RTO charges.
  - **Example Calculation**:
    - If `slab_count` = 2 (implies 2 x 0.5 KG slabs):
      - For `Order ID` = 2001806232, `slab_weight` = 1.5 KG, `slab_count` = 3, `Type of shipment` = Forward, `Zone` = d:
        - Total Charge = `fwd_fixed` + (2 * `fwd_additional`) = 45.4 + (2 * 44.8) = 135.

### Step 4: Data Merging

- Merge LHS and RHS data to create a comprehensive dataset.

### Step 5: Data Extraction

- Extract the following:
  - **Order ID**
  - **AWB Number**
  - **Total Weight as per X (KG)**
  - **Weight Slab as per X (KG)**
  - **Total Weight as per Courier Company (KG)**
  - **Weight Slab Charged by Courier Company (KG)**
  - **Delivery Zone as per X**
  - **Delivery Zone Charged by Courier Company**
  - **Expected Charge as per X (Rs.)**
  - **Charges Billed by Courier Company (Rs.)**
  - **Difference Between Expected Charges and Billed Charges (Rs.)**

### Step 6: Reporting

- Generate reports highlighting discrepancies, including overcharges and undercharges.

### Step 7: Data Saving

- Save the cleaned and merged data to a CSV file for further analysis and visualization in Power BI.

---

## Design Considerations

- **Data Integrity**: Ensure all data sources are accurately mapped and processed.
- **Scalability**: Design the analysis to handle large volumes of data efficiently.
- **Visualization**: Prepare reports that clearly highlight discrepancies for easy interpretation.

---

## Usage

1. **Prepare Data**:
   - Ensure input data (website order report, SKU master, and warehouse pincode mapping) and RHS data (courier company invoice) are available in the required format.

2. **Run Analysis**:
   - Execute the provided Python scripts to process the data and calculate the charges.

3. **Review Results**:
   - Analyze the output to identify any discrepancies and take corrective actions as needed.
