# Listing standards

## Contents

- Facts first
- What to remove from supplier listings
- Variants and sizes
- Information some markets require
- Writing
- Published products: a Shopify CSV

## Facts first

Work from the product facts: the supplier title, description, specifications, images, and variants from DSers, plus what the user tells you. Ask for the store's language, market, and tone if they are not clear.

- Use only facts you have. Do not add materials, measurements, certifications, approvals, or claims such as "waterproof", "non-toxic", "BPA-free", "FDA approved", or "CE certified" unless the facts state them and the supplier can document them. Do not write medical claims such as "relieves pain" or "treats". If a fact is missing, ask or leave it out.
- List the facts you need the user to confirm.

## What to remove from supplier listings

**Title**: years and "New", "Hot Sale", "Free Shipping", "Dropshipping", "1pc", words in a language the store does not use, repeated synonyms, and other brands' names. Keep another brand's name only to state compatibility, written as "compatible with [model]", never in a way that suggests the product comes from or is endorsed by that brand. Google Merchant Center also rejects promotional text, price, sale, and shipping information, and capitals for emphasis in titles (see [rules.md](rules.md)).

**Description**: notes from the supplier to its own buyers ("Dear buyer", requests for feedback, "contact us for wholesale"), the supplier's shipping, payment, and return terms, Chinese characters, size charts as images with Chinese text, machine-translated sentences, and repeated warranty blocks.

**Images**: watermarks, supplier logos, text and promotional overlays, and other brands' logos. Google rejects watermarks and promotional overlays (see [rules.md](rules.md)). Use only images the seller has the right to use. You cannot edit image files; list the images to drop or replace, and reorder the rest so the clearest product photo comes first.

## Variants and sizes

- Rename supplier codes such as "Color: A, B, C" or "Red#01" to real names, using each variant's photo. Merge variants that are the same.
- A "Ships From" option (for example China and United States) usually means different costs and delivery times. If the store will sell from one location, remove the other values; if both stay, price them separately and show each delivery time.
- Clothing and shoe sizes from Asian suppliers often run smaller than US or EU sizes with the same letter. Build the size chart from the supplier's measurements in centimetres, convert for US stores (1 inch = 2.54 cm), and tell customers to compare with a garment they own.
- Convert other units to the ones the store's customers use, and keep both where helpful.
- Rename variants in the Import List, before publishing. Changing variant options in the store after publishing can break the link to the supplier.

## Information some markets require

- **EU**: an online offer to EU consumers must show the manufacturer's name and postal and electronic address; if the manufacturer is outside the EU, the EU responsible person's name and addresses; information identifying the product; and warnings or safety information in the local language (see [rules.md](rules.md)). Ask the supplier for these. If there is no responsible person in the EU, tell the user the product may not be placed on the EU market until there is one.
- **US children's products**: these need a Children's Product Certificate (see [rules.md](rules.md)). Ask the supplier for it before listing.

## Writing

**Title**: start with what the product is, in the words shoppers search for, then its main difference, such as material, size, or use. Put the important words first, because search results and shopping ads often cut long titles.

**Description**:

1. One or two sentences on the main benefit.
2. Bullet points that turn each feature into what it does for the customer.
3. Specifications: size, material, weight, and variants.
4. What is in the box.
5. Care, sizing, compatibility, or usage notes where relevant.

Match the store's tone and keep it easy to scan. Do not promise delivery times the seller cannot control. Keep the format of the current description: if it is HTML, use simple HTML (paragraphs, lists, a table for sizes).

**URL handle and tags**: a short lowercase handle with hyphens and the main search term; tags the store actually uses for filtering and collections. Suggest keywords from the product facts; do not state search volumes unless the user provides keyword data.

## Published products: a Shopify CSV

DSers tools do not edit the text of products already in the store. For one product, give each field ready to paste into the store admin. For many Shopify products:

1. Ask the user to export the products from their Shopify admin as a CSV, and work from that file. Column names have changed over time, so keep the header names exactly as they are in the export.
2. Return a file with one row per product containing only the URL handle, the Title, and the columns you changed (for example Description, SEO title, SEO description, Tags). Columns left out of the file stay as they are in the store.
3. Fill every cell in the columns you include. A blank cell erases the store's value; copy the current value where nothing changes.
4. Do not include option or variant columns. Changing option values gives the variants new IDs, which can break the link to the supplier, and variant columns without the option columns replace the existing variants (see [rules.md](rules.md)).
5. Tell the user to keep the original export as a backup, import with "Overwrite products with matching handles", and try two or three products first.
