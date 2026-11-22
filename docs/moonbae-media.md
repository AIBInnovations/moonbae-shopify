# Moonbae campaign and product imagery

The theme contains responsive campaign banners and twelve bundled fallback JPEGs. Product model photos remain in the store's Shopify Files library and are associated with their original products. The [media manifest](moonbae-media-2026-11-22.json) records their file IDs, CDN URLs, product IDs, and audience selections.

Men and Women collection links retain `?view=men` or `?view=women` when opening a product. The matching alternate product templates preserve the default product-page configuration. Cards and product pages select the product's `moonbae.model_image_men` or `moonbae.model_image_women` metafield, each a Shopify `file_reference` pointing to a MediaImage. Size changes preserve this selected image; audience cards keep it on hover.

All 23 unisex products have separate male and female model images. The nine baby tees use female models. Ordinary catalogue views use the product's featured model photo. Generated model photos precede the original packshots in each product's media order; the original files and their relative order remain intact.

The 2026-11-22 update added 115 reviewed model images, giving the 32 active products 179 images in total and at least five each. Three existing terry tees were active but unpublished to any sales channel at verification; their images and audience mappings are prepared in Admin.

Validation covered live desktop, tablet and mobile card alignment, responsive full-width banners, static Basics images, Men/Women cards and product pages, hover, size changes, recommendations and navigation. Local working images, prompts, screenshots and research documents remain under the ignored `output/` directory. The market report was delivered separately to the owner's Desktop.
