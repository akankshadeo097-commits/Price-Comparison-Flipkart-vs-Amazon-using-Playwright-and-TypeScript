{
  "name": "price-comparator",
  "version": "1.0.0",
  "scripts": {
    "test": "npx playwright test"
  },
  "devDependencies": {
    "@playwright/test": "^1.44.1",
    "typescript": "^5.4.0"
  }
}

2. tsconfig.json

{
  "compilerOptions": {
    "target": "ESNext",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true
  },
  "include": ["tests"]
}

3. playwright.config.ts

import { defineConfig } from '@playwright/test';

export default defineConfig({
  use: {
    headless: true,
    viewport: { width: 1280, height: 720 },
    ignoreHTTPSErrors: true,
  },
  timeout: 60000,
});

4. tests/comparePrices.spec.ts

import { test, expect } from '@playwright/test';

test('Compare price of iPhone 15 Plus on Flipkart and Amazon', async ({ browser }) => {
  const flipkartCtx = await browser.newContext();
  const amazonCtx = await browser.newContext();

  const flipkartPage = await flipkartCtx.newPage();
  const amazonPage = await amazonCtx.newPage();

  const product = 'iphone 15 plus';
  let flipkartPrice: number = 0;
  let amazonPrice: number = 0;

  await Promise.all([
    (async () => {
      await flipkartPage.goto('https://www.flipkart.com');
      expect(flipkartPage.url()).toContain('flipkart');
      expect(await flipkartPage.title()).toContain('Flipkart');

      try {
        await flipkartPage.locator('button:has-text("✕")').click({ timeout: 3000 });
      } catch {}

      await flipkartPage.locator('input[title="Search for products, brands and more"]').fill(product);
      await flipkartPage.keyboard.press('Enter');
      await flipkartPage.waitForTimeout(3000);

      const firstResult = flipkartPage.locator('div._4rR01T').first();
      await expect(firstResult).toContainText('iPhone');

      const priceText = await flipkartPage.locator('div._30jeq3').first().innerText();
      flipkartPrice = parseInt(priceText.replace(/[^0-9]/g, ''));
      console.log(`Flipkart Price: ₹${flipkartPrice}`);
    })(),

    (async () => {
      await amazonPage.goto('https://www.amazon.in');
      expect(amazonPage.url()).toContain('amazon');
      expect(await amazonPage.title()).toContain('Amazon');

      await amazonPage.locator('input#twotabsearchtextbox').fill(product);
      await amazonPage.keyboard.press('Enter');
      await amazonPage.waitForTimeout(3000);

      const firstResult = amazonPage.locator('span.a-size-medium').first();
      await expect(firstResult).toContainText('iPhone');

      const priceText = await amazonPage.locator('span.a-price-whole').first().innerText();
      amazonPrice = parseInt(priceText.replace(/[^0-9]/g, ''));
      console.log(`Amazon Price: ₹${amazonPrice}`);
    })()
  ]);

  expect(flipkartPrice).toBeLessThan(amazonPrice);

  console.log(`✅ Test Passed: Flipkart (₹${flipkartPrice}) < Amazon (₹${amazonPrice})`);
});

How to Run

1. Install dependencies: `npm install`
2. Install Playwright browsers: `npx playwright install`
3. Run the test: `npx playwright test`

Test Failure Example

If Flipkart price is higher than Amazon, you’ll see:

Error: expect(received).toBeLessThan(expected)
Received: 84999
Expected: < 79999

