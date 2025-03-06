# QA Test Report - Purchase Assistant

## Introduction  
This document describes the Quality Assurance (QA) tests conducted on the **Purchase Assistant**, an AI-based shopping assistant that interacts with users and provides product-related information using the OpenAI API.  

The objective of this report is to validate the correct functionality of the assistant, identify potential errors, and ensure robustness against different input scenarios.  

## Test Environment  
- **Operating System:** Windows 10  
- **Programming Language:** Python 3.8+  
- **Dependencies:**  
  - OpenAI API  
  - dotenv  
  - typing_extensions  
- **Required Files:** `catalog.json`, `.env`  
- **Environment Variable:** `OPENAI_API_KEY` properly configured  
- **Repository:** [GitHub link]  

---

## 1. Function `get_all_products`  
**Objective:** Ensure that the assistant returns the list of products.  
**Test:** Asked `"What products are available?"`.  
**Expected Result:** It should display all products from `catalog.json`.  
**Actual Result:** The assistant correctly retrieved and displayed the full list of products.  

#### Chat Output  
After completing the flow and requesting the available products, the assistant correctly retrieves and displays the full list from `catalog.json`:  

![Assistant Response - All Products](screenshots/todosproductos.JPG)  

#### Verification of `catalog.json`  
To confirm that the assistant is pulling data correctly, we checked `catalog.json` and verified that all products are properly stored:  

![Catalog Data - Page 1](screenshots/Catalog1.JPG)  
![Catalog Data - Page 2](screenshots/Catalog2.JPG)  
![Catalog Data - Page 3](screenshots/Catalog3.JPG)  


Trying show a inexistent product similar to a product available the flow is showing correctly 

![Test with unavailable product](screenshots/iphone14test.JPG)  

#### **Test with an Empty Catalog**  
When `catalog.json` was empty (`[]`), the assistant returned an unclear error message:  

![Error when catalog is empty](screenshots/emptycatalog.JPG)  
 
Unclear message from the assitance:

![Unclear message](screenshots/emptycatalogchat.JPG) 

---

## 2. Function `get_product_info`  
**Objective:** Validate that the assistant provides correct product information.  
**Test:** Asked `"I want information about Apple iPhone 13."`  
**Expected Result:** The assistant should return the product name, description, and price from `catalog.json`.  
**Actual Result:** The assistant correctly retrieved and displayed the information.  

#### Chat Output  
After requesting details for the iPhone 13, the assistant provided the expected response:  

![Product Info - iPhone 13](screenshots/getinfo.JPG)

### 2.1 Handling Non-Existent Products  
**Objective:** Verify that the assistant correctly handles requests for products that are not in `catalog.json`.  
**Test:** Asked `"I want information about Samsung Galaxy S1000."`  
**Expected Result:** The assistant should respond with `"Product not found."` or suggest an alternative.  
**Actual Result:** The assistant provided a clear response indicating that the product was not available and suggested an alternative (Samsung Galaxy S21).  

#### Chat Output  
When requesting information about a non-existent product, the assistant handled it correctly:  

![Product Not Found](screenshots/inexistentproduct.JPG)  

### 2.2 Handling Case Sensitivity and Minor Typos  
**Objective:** Verify that the assistant correctly recognizes product names regardless of case sensitivity and small typos.  
**Test:** Asked for `"apple iphone 13"` using different capitalizations and a minor typo.  
**Expected Result:** The assistant should recognize `"Apple iPhone 13"` correctly in all cases.  
**Actual Result:** The assistant successfully retrieved product information in all cases, including when using:  

1️ **All lowercase:** `"i want information about apple iphone 13"`  
2️ **All uppercase:** `"I WANT INFORMATION ABOUT APPLE IPHONE 13"`  
3️ **Minor typo:** `"i want information about aplle iphone 13"`  

#### Chat Output  
The assistant correctly handled case variations and a minor typo in a single test session:  

![Case Sensitivity and Typo Test](screenshots/typostest.JPG)  

---

## 3. Function `get_product_stock`  
**Objective:** Confirm that product stock is displayed correctly.  
**Test:** Asked `"How many PlayStation units are available?"`.  
**Result:** `"There are currently 20 units available of the Sony PlayStation 5."`  

---

## 4. Full Interaction Flow  
**Objective:** Simulate a complete user session to verify the assistant’s ability to process multiple requests seamlessly.  

### **Test Scenario:**  
1. User: `"I'm looking for a PlayStation 5"`  
2. Assistant: `"I'm searching..."`  
3. Assistant initially failed to retrieve PlayStation 5 information despite being in the catalog (error).  
4. User: `"What products are available?"`  
5. Assistant correctly lists all available products.  
6. User: `"How many iPhone 13 units do you have?"`  
7. Assistant correctly responds: `"25 units available."`  
8. User: `"How many OnePlus 9 Pro units are available?"`  
9. Assistant correctly responds: `"22 units available."`  
10. User: `"I need 20 units."`  
11. Assistant: `"You can proceed with your purchase."`  

### **Screenshots:**  
#### **Before Fix (Incorrect Response for PlayStation 5)**  
![Chat Flow Error](screenshots/chat_flow_error.png)  

#### **After Fix (Correct Stock Response for All Products)**  
![Chat Flow Success](screenshots/chat_flow_success.png)  

---

| **Error** | **Description** | **Solution** |  
|-----------|---------------|-------------|  
| Empty input | Entering a blank message caused `BadRequestError` | Added input validation to ignore empty messages |  
| Product not found | Searching for `"TabletXYZ"` returned an unhandled error | Implemented a default response `"Product not found"` |  
| Empty catalog | If `catalog.json` is empty (`[]`), the assistant responds with `"It seems there was an issue retrieving the list of available products."` instead of `"No products available."` | Improved error handling to return a clearer message when `catalog.json` is empty |


---

## Improvement Recommendations  
- **Better validation of product names** to avoid search errors.  
- **Clearer response formatting** (e.g., ordered lists in output).  
- **Handling of code injection attempts** for security.  

---

## Submission Evidence  
- **GitHub Repository:** [Link to the repo]  
- **Submission Video:** [Link to the video]  
