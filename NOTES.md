# NOTES.md

## What changed

- Reworked the storefront styling to follow the supplied BRAND.md direction: calm tea-green, cream and parchment tones, two-font typography, responsive layout, accessible controls, and subtle motion.
- Removed the original jQuery, Font Awesome and animate.css dependencies and used plain HTML/CSS/JavaScript.
- Fixed cart initialization so a first visit starts with an empty cart.
- Fixed Add to Cart so prices come from `PRODUCTS`, sold-out products cannot be added, and one product cannot exceed 5 units or available stock.
- Fixed cart count to show total quantity rather than number of distinct products.
- Fixed cart plus/minus/remove behavior and prevented invalid quantities.
- Fixed coupon handling for `WELCOME10`: case-insensitive input, minimum ₹399 subtotal, 10% discount, maximum ₹150 discount, Gifts excluded, and one coupon per order.
- Fixed shipping calculation so the free-shipping check uses the amount after discount. The original implementation used ₹599 as the threshold; the assessment brief names a free-shipping threshold but does not specify its numeric value, so ₹599 was retained as a temporary assumption.
- Fixed category filtering so All, Black, Green, Herbal and Gifts work.
- Fixed search so API search results are matched correctly and search, category and sorting work together.
- Fixed sorting for price low-to-high, price high-to-low and name, with sold-out products kept last.
- Fixed Quick View product selection.
- Added proper sold-out presentation and disabled Add to Cart for sold-out items.
- Fixed pincode validation and rejection handling so invalid/non-serviceable checks do not remain stuck on “Checking...”.
- Reworked newsletter validation and success feedback without an automatic popup.
- Preserved the supplied `PRODUCTS` ids, names and prices.
- Preserved the supplied API block and checkout form contract.
- Preserved the supplied legal wording.

## AI mistakes / how they were caught

The original starter code contained several deliberate/observed problems that were caught during testing and source review:

- `cart` could be `null` on first load.
- Product price was read from rendered DOM text instead of the source product data.
- Quantity could grow beyond stock and beyond the required maximum of 5.
- Sold-out products still exposed an Add to Cart action.
- Cart count used the number of cart entries rather than total quantity.
- The original category filter treated `all` as a real category, so All returned no products; Green also used the wrong case.
- Sort comparators returned booleans instead of numeric comparison results.
- Search used `indexOf()` as a truthy test, which could exclude/mis-handle matching IDs.
- Quick View used a loop-variable closure that could point at the wrong product.
- Cart event listeners were added every time the cart was rendered.
- Quantity `+` could concatenate strings instead of adding numbers.
- Remove used `splice(i)` and could remove more than one item.
- Coupon logic did not implement all required business rules.
- Pincode rejection had no catch path, so invalid input could remain on “Checking...”.
- The original countdown used a date that had already passed.
- The original newsletter popup appeared automatically, contrary to the supplied brand guidance.

These were checked against the supplied assessment requirements and then re-tested in the browser.

## Testing

The following tests were reported as passing during the final browser check:

- Cart add/open/quantity/remove
- Maximum quantity and stock handling
- Sold-out product handling
- Search
- Category filters
- Sorting
- Quick View
- Valid pincode
- Invalid pincode handling
- Mobile layout / no sideways scrolling

The mobile check also confirmed that the header fit, product cards used two columns, and text/buttons were not clipped.

## Images

The existing image paths were retained:

- `images/logo.png`
- `images/hero-banner.png`
- `images/p101.png` through `images/p108.png`

No claim is made here that new AI-generated product images were created during this work.

## Assumptions / open questions

- The assessment brief requires free shipping at a stated threshold but does not give the numeric threshold in the business-rule text. The original implementation used ₹599, so that value was retained as a temporary assumption.
- The supplied source contains an external example checkout URL. The checkout form contract was preserved as required; the brief states that the URL does not exist and an error page is expected.

## Time spent

Not recorded precisely.

## With more time

- Create and document the required image-generation prompts and produce final optimized WebP/AVIF product and hero assets if the assessment still requires replacement of the starter images.
- Run a formal Lighthouse pass and record the results.
- Test in multiple browsers and on a physical phone.
- Package the final project with `images/`, `NOTES.md` and `PROMPTS.md`, then verify the ZIP contents and size before submission.
