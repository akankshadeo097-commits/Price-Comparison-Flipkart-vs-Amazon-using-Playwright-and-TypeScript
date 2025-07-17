# Price-Comparison-Flipkart-vs-Amazon-using-Playwright-and-TypeScript
Compare Product Price on Flipkart and Amazon using Playwright with 
TypeScript 
Objective: 
Write an end-to-end test using Playwright and TypeScript that: 
1. Navigate to Flipkart and Amazon  
2. Validate url and title of the page 
3. Searches for a given product name on both Flipkart and Amazon. 
4. Validates that the product appears in the search results and pick the same product in 
both the site 
5. Extracts the price of the first matching product from both websites. 
6. Compares the prices: 
○ If the Flipkart price is less than the Amazon price, the test passes. 
○ If not, the test fails with a message showing prices on both sites. 
�
�
 Product to Search: "iphone 15 plus" 
✅
 Acceptance Criteria: 
● Use Playwright with TypeScript 
● Use parallel execution to open both websites and search simultaneously 
● Extract price of the product being compared 
● Add multiple assertion 
● Print prices in console 
● If the test fails, provide a cl
