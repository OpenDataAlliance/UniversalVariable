# Open Data Alliance Universal Variable Specification Version 1.1.0

This specification describes a way to structure information commonly found on commercial web sites into a JavaScript object in a web page.  This object can then be read by code running in a user's web browser (e.g. JavaScript 'tags', browser extensions) or by other consumers of web pages (e.g. automatic crawlers).


## Namespace

Our namespace is `window.universal_variable.*`:

Example:

``` javascript
window.universal_variable = {};
```

## The universal_variable object

universal_variable can contain any of the following properties:


<table>
	<tr><th>JavaScript key</th><th>Type</th><th>Describes</th></tr>
	<tr><td>user</td><td><a href="#user">User object</a></td><td>The visitor or logged in user.</td></tr>
	<tr><td>page</td><td><a href="#page">Page object</a></td><td>The page currently being viewed.</td></tr>
	<tr><td>product</td><td><a href="#product">Product object</a></td><td>The product being shown on this page, if a single product is being displayed.</td></tr>
	<tr><td>basket</td><td><a href="#basket">Basket object</a></td><td>The state of the visitor's basket at the time this page was served.</td></tr>
	<tr><td>transaction</td><td><a href="#transaction">Transaction object</a></td><td>A transaction that has <i>just completed</i> (if this is the first page view served to the user since they completed the transaction).</td></tr>
	<tr><td>listing</td><td><a href="#listing">Listing object</a></td><td>Multiple products that are present on a page, excluding recommendations (e.g. search results, or a product category page).</td></tr>
	<tr><td>recommendation</td><td><a href="#recommendation">Recommendation object</a></td><td>Products that are recommended to the user on this page.</td></tr>
	<tr><td>events</td><td><a href="#eventlist">EventList object</a></td><td>Identifies events that have just occurred.</td></tr>
	<tr><td>version</td><td>String</td><td>Which version of this standard is being used, currently '1.1.0'.</td></tr>
</table>

## Only declare what's necessary

The universal variable object should be declared _lazily_: only include a property if it's actually necessary.  

If a web site does not implement product recommendations, the window.universal_variable.recommendation object should not even be declared.

The same applies to properties of objects.  For example, if a page has categories but not subcategories, simply leave out the 'subcategory' property, do not declare it with any value.

A very simple example of a universal_variable object would be:

``` javascript
window.universal_variable = {
	user: {
		returning: "true"
	},
	page: {
		category: "home"
	},
	version: "1.1.0"
}
```

## Implementation Considerations

This universal_variable JavaScript object must be created before any browser scripts that try to access it.  





## Version

Set this to the string "1.1.0" to indicate that this version of the specification is being used.

```javascript
window.universal_variable.version = "1.1.0";
```

## Page

The Page object describes the current page.

Properties (all optional):

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Page Category</td><td>category</td><td>String</td><td>A short description of the type of page, e.g. 'home', 'product', 'category', 'search', 'basket', 'checkout', 'confirmation'.</td></tr>
<tr><td>Page Subcategory</td><td>subcategory</td><td>String</td><td>A short description of the type of page, with more granularity than the category, e.g. 'landing', 'checkout-stage1', 'gifts'.  <br>Use only if a category has been defined.</td></tr>
<tr><td>System Environment</td><td>environment</td><td>String</td><td>A name for the environment which is creating this Universal Variable data, e.g. 'development', 'testing', 'production'.</td></tr>
<tr><td>Page Variation</td><td>variation</td><td>String</td><td>If serving multiple versions of this page during testing, specify a variation name. e.g. 'original','newstyle'</td></tr>
<tr><td>Page Revision</td><td>revision</td><td>String</td><td>If a site's pages implement versioning, state a revision here, e.g. '1.1'.</td></tr>
</table>

For example:

``` javascript
window.universal_variable = {
	page: {
		category: "product",
		subcategory: "Mens Shirts",
		environment: "production",
		variation: "Original",
		revision: "1.1"
	}
}
```

## User

The User object describes the current user of the web site.

Properties (all optional):

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>User Real Name</td><td>name</td><td>String</td><td>The user's full name.</td></tr>
<tr><td>User Login Name</td><td>username</td><td>String</td><td>The identifier that the user provides to log in to the site (the 'username').<br>Use only if a category has been defined.</td></tr>
<tr><td>User Internal ID</td><td>user_id</td><td>String</td><td>A unique identifier that the web site uses internally to identify this user.</td></tr>
<tr><td>User Email Address</td><td>email</td><td>String</td><td>The user's full email address.</td></tr>
<tr><td>User Preferred Language</td><td>language</td><td>String</td><td>The user's preferred language, must be an <a href="http://en.wikipedia.org/wiki/IETF_language_tag">IETF compatible string</a>, e.g. 'en-us', 'en-gb'.  IETF codes start with an <a href="http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes">ISO 639-1</a> language representation, and are extensible by region.</td></tr>
<tr><td>User Returning Status</td><td>returning</td><td>Boolean</td><td>False if this page view forms part of the user's first visit to this site, True otherwise.</td></tr>
<tr><td>User Types</td><td>types</td><td>Array of Strings</td><td>Arbitrary labels to assign to this user, e.g. 'high-value','female'.</td></tr>
<tr><td>User Facebook ID</td><td>facebook_id</td><td>Number</td><td>The user's Facebook User ID, as returned by the Facebook API.</td></tr>
<tr><td>User Twitter ID</td><td>twitter_id</td><td>String</td><td>The user's Twitter ID.</td></tr>
</table>

Example:

``` javascript
window.universal_variable = {
	user: {
		name: "Example User",
		username: "exampleuser123",
		user_id: "8492834083",
		email: "user@example.com",
		language: "en-gb",
		returning: true,
		types: ['high-value','female']
		facebook_id: 12345678901232345,
		twitter_id: "myid"
	}
}
```

## Product

The Product object describes a single product.

This object can:

* Be a property of the universal_variable object, where one product is displayed on the page.
* Be used as part of another Product object to denote linked products (see below).
* Form part of the [Listing object](#listing) if several products are present on the page.
* Form part of the [Recommendation object](#recommendation) if the page contains product recommendations.
* Form part of the [LineItem object](#lineitem) as part of a transaction or basket.

There are many possible types of product on the Web - here, we first list properties which could reasonably apply to any product, and then list additional properties which could be declared for certain kinds of product.  In any case, the properties listed below are all optional.

### Properties common across most products

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Product ID</td><td>id</td><td>String</td><td>A unique identifier for the product, that is used by the web site only, i.e. not necessarily a Stock Keeping Unit (SKU) Code.</td></tr>
<tr><td>Product URL</td><td>url</td><td>String</td><td>A canonical URL for this product.</td></tr>
<tr><td>Product Name</td><td>name</td><td>String</td><td>Name of the product.</td></tr>
<tr><td>Product Description</td><td>description</td><td>String</td><td>Brief description of the product.</td></tr>
<tr><td>Product Manufacturer</td><td>manufacturer</td><td>String</td><td>Name of the manufacturer for this product.</td></tr>
<tr><td>Product Category</td><td>category</td><td>String</td><td>A short description of this type of product, e.g. 'shoes', 'package holiday'.</td></tr>
<tr><td>Product Subcategory</td><td>subcategory</td><td>String</td><td>A short description of this type of product, with more granularity than the category, e.g. 'trainers'. <br>Use only if a category has been defined.</td></tr>
<tr><td>Product Linked Products</td><td>linked_products</td><td>Array of <a href="#product">Product</a> objects</td><td>Products related to this one through well-defined relationships (e.g. a product in the same range from the same manufacturer), not generated based on the output of recommendation algorithms.</td></tr>
<tr><td>Product Currency</td><td>currency</td><td>String</td><td>The <a href="http://en.wikipedia.org/wiki/ISO_4217">ISO 4217</a> code for the currency used for this product's prices.</td></tr>
<tr><td>Product Price</td><td>unit_sale_price</td><td>Number</td><td>The price for a single unit of this product actually paid by a customer, taking into account any sales and promotions.  <i>Requires Product Currency to be declared.</i></td></tr>
<tr><td>Product Price Excluding Promotions</td><td>unit_price</td><td>Number</td><td>The price of a single unit of this product, not taking into account discounts and promotions. <i>Requires Product Currency and Product Price to be declared.</i></td></tr>

</table>

### Additional properties for products requiring stock keeping

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Product SKU Code</td><td>sku_code</td><td>String</td><td>The Stock Keeping Unit (SKU) code for the product.</td></tr>
<tr><td>Product Stock Remaining</td><td>stock</td><td>Number</td><td>The quantity of this product remaining in stock (zero for out-of-stock).</td></tr>
</table>

### Additional properties for products with promotions

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>

<tr><td>Product Voucher Code</td><td>voucher</td><td>String</td><td>A voucher code that has been entered by the user which changes the price of this product.  If the user's voucher is not product-specific, it should instead be applied to the Transaction object after a transaction has been completed.</td></tr>
</table>

### Additional properties for products that have variations chosen by the user

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Product Color</td><td>color</td><td>String</td><td>The currently selected color of this product.</td></tr>
<tr><td>Product Size</td><td>size</td><td>String</td><td>The currently selected size of this product.</td></tr>
</table>

### Additional properties for travel-related products

As before, if some properties are not known at the current stage in a user's journey, such as checkin and checkout dates, simply do not declare them.

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Journeys</td><td>journeys</td><td>Array of <a href="#journey">Journey</a> objects</td><td>Descriptions of the flights, trains, or other journeys included in this product.</td></tr>
<tr><td>Accommodations</td><td>accommodations</td><td>Array of <a href="#accommodation">Accommodation</a> objects</td><td>Descriptions of the accommodation stays included in this product.</td></tr>
</table>

See the following example of a populated Product object:

```javascript
{

	id: "ABC123",
	sku_code: "123",
	url: "http://www.example.com/product?=ABC123", 
	name: "XYZShoes",
	description: "most popular shoes in our shop",
	manufacturer: "Acme Corp",
	category: "Shoe",
	subcategory: "Trainers",
	linked_products: [Product, Product, Product, ...],
	color: "WHITE",
	size: "M",
	stock: 10,
	unit_price: 123.00,
	unit_sale_price: 100.00,
	currency: "GBP",
	voucher: "MYVOUCHER"
}
```


## LineItem

The LineItem object describes a quantity of [Products](#product).  Arrays of LineItems are used as part of a [Basket](#basket) or [Transaction](#transaction).

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>LineItem Product</td><td>product</td><td><a href="#product">Product</a> object</td><td><i>Mandatory.</i> The product which has been added to the basket or transaction.</td></tr>
<tr><td>LineItem Quantity</td><td>quantity</td><td>Number</td><td><i>Mandatory.</i> The number of this product that has been added to the basket or transaction.</td></tr>
<tr><td>LineItem Subtotal</td><td>subtotal</td><td>Number</td><td>Total cost of this LineItem, including tax, excluding shipping.</td></tr>
</table>

Example:

```javascript
{
	product: {
			url: "http://www.example.com/product?=ABC123", 
			name: "ABC Trainers",
			unit_price: 30.00
			currency: "GBP"
		},
	quantity: 1,
	subtotal: 30.00
}
```

## Basket

The Basket object describes the current state of the a user's shopping basket or cart.

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Basket ID</td><td>id</td><td>String</td><td>A unique ID for this basket.</td></tr>
<tr><td>Basket Currency</td><td>currency</td><td>String</td><td><i>Mandatory.  </i>The <a href="http://en.wikipedia.org/wiki/ISO_4217">ISO 4217</a> code for the currency this basket's costs are denominated in.</td></tr>
<tr><td>Basket Price</td><td>subtotal</td><td>Number</td><td><i>Mandatory.  </i>The cost of the basket, excluding shipping or discounts.</td></tr>
<tr><td>Basket Price Includes Tax</td><td>subtotal_include_tax</td><td>Boolean</td><td>Indicates whether Basket Price includes tax.</td></tr>
<tr><td>Basket Voucher Code</td><td>voucher</td><td>String</td><td>Voucher code entered.</td></tr>
<tr><td>Basket Voucher Discount</td><td>voucher_discount</td><td>Number</td><td>Total amount of discount due to the voucher code entered.</td></tr>
<tr><td>Basket Tax</td><td>tax</td><td>Number</td><td>The amount of tax payable for this basket.</td></tr>
<tr><td>Basket Shipping Cost</td><td>shipping_cost</td><td>Number</td><td>The amount of shipping cost payable for the items in this basket.</td></tr>
<tr><td>Basket Shipping Method</td><td>shipping_method</td><td>String</td><td>Delivery method selected for the items in this basket.</td></tr>
<tr><td>Basket Total</td><td>total</td><td>Number</td><td>The total cost of this basket, including tax, shipping and discounts.</td></tr>
<tr><td>Basket Items</td><td>line_items</td><td>Array of <a href="#lineitem">LineItem</a> objects</td><td>The items (and their quantities) present in the basket.  One LineItem per distinct product.</td></tr>
</table>

Example:

``` javascript
window.universal_variable = {
	basket: {
		id: "BASKET2203",
		currency: "GBP",
		subtotal: 123.00,
		subtotal_include_tax: true,
		tax: 12.00,
		shipping_cost: 1.00,
		shipping_method: "Standard Mail",
		total: 123.00,
		line_items: [LineItem, LineItem, LineItem, ...]
	}
}
```

## Address

The Address object is used for billing and shipping information in the [Transaction](#Transaction) object.

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Name</td><td>name</td><td>String</td><td>Full name of the recipient.</td></tr>
<tr><td>Address</td><td>address</td><td>String</td><td>Street address (excluding city, state, postcode, country).</td></tr>
<tr><td>City</td><td>city</td><td>String</td><td>City.</td></tr>
<tr><td>State</td><td>state</td><td>String</td><td>State (Two-letter abbreviation if a US state)</td></tr>
<tr><td>Postcode</td><td>postcode</td><td>String</td><td>The postal code or ZIP code.</td></tr>
<tr><td>Country</td><td>country</td><td>String</td><td>Country, using the two-letter <a href="http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2">ISO 3166-1 alpha 2</a> standard.</td></tr>
</table>

Example:

``` javascript
{

	name: "My Name",
	address: "234 High Street",
	city: "London",
	state: "London",
	postcode: "SW1 1AB",
	country: "GB"
}
```

## Transaction

The Transaction object describes a completed purchase.  If possible, this object should only be present when the transaction has just been completed and is no longer modifiable.

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Transaction ID</td><td>order_id</td><td>String</td><td>A unique ID for this transaction.</td></tr>
<tr><td>Transaction Currency</td><td>currency</td><td>String</td><td><i>Mandatory.  </i>The <a href="http://en.wikipedia.org/wiki/ISO_4217">ISO 4217</a> code for the currency this transaction's costs are denominated in.</td></tr>
<tr><td>Transaction Payment Type</td><td>payment_type</td><td>String</td><td>Payment method, e.g. 'Visa','PayPal','Voucher'.</td></tr>
<tr><td>Transaction Price</td><td>subtotal</td><td>Number</td><td>The transaction amount, excluding shipping or discounts.</td></tr>
<tr><td>Transaction Includes Tax</td><td>subtotal_include_tax</td><td>Boolean</td><td>Indicates whether Transaction Price includes tax.</td></tr>
<tr><td>Transaction Voucher Code</td><td>voucher</td><td>String</td><td>Voucher code entered.</td></tr>
<tr><td>Transaction Voucher Discount</td><td>voucher_discount</td><td>Number</td><td>Total amount of discount due to the voucher code entered.</td></tr>
<tr><td>Transaction Tax</td><td>tax</td><td>Number</td><td>The amount of tax payable for this transaction.</td></tr>
<tr><td>Transaction Shipping Cost</td><td>shipping_cost</td><td>Number</td><td>The amount of shipping cost payable for this transaction.</td></tr>
<tr><td>Transaction Shipping Method</td><td>shipping_method</td><td>String</td><td>Delivery method selected for this transaction.</td></tr>
<tr><td>Transaction Total</td><td>total</td><td>Number</td><td><i>Mandatory. </i>The total cost of this transaction, including tax, shipping and discounts.</td></tr>
<tr><td>Transaction Delivery Address</td><td>delivery</td><td><a href="#address">Address</a> object</td><td>Delivery address for this transaction.</td></tr>
<tr><td>Transaction Billing Address</td><td>billing</td><td><a href="#address">Address</a> object</td><td>Billing address for this transaction.</td></tr>
<tr><td>Basket Items</td><td>line_items</td><td>Array of <a href="#lineitem">LineItem</a> objects</td><td>The items (and their quantities) present in the basket.  One LineItem per distinct product.</td></tr>
</table>

Example:

```javascript
window.universal_variable = {
	transaction: {
		order_id: "WEB123456",
		currency: "GBP",
		subtotal: 123.00,
		subtotal_include_tax: true,
		voucher: "MYVOUCHER",
		voucher_discount: 0.00,
		tax: 10.00,
		shipping_cost: 1.00,
		shipping_method: "Standard Mail",
		total: 130.00,

		delivery: {
			name: "Full Name",
			address: "234 High Street",
			city: "London",
			state: "London",
			postcode: "SW1 1AB",
			country: "GB",
		},

		billing: {
			name: "Full Name",
			address: "234 High Street",
			city: "London",
			state: "London",
			postcode: "SW1 1AB",
			country: "GB",
		},

		line_items: [LineItem, LineItem, LineItem, ...]
	}
}
```

## Listing

The listing object describes a list of [Products](#product), for example as displayed as part of category page or search results page. 

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Listing Search Query</td><td>query</td><td>String</td><td>If the products are search results, the query that was entered.</td></tr>
<tr><td>Listing Items</td><td>items</td><td>Array of <a href="#product">Product</a> objects</td><td>The products which have been displayed to the user on this page.</td></tr>
</table>

Example:

```javascript
window.universal_variable = {
	listing: {
		query: "shoes on sale",
		items: [Product, Product, Product, ...]
	}
}
```

## Recommendation

The Recommendation object describes products that have been recommended on a page, based on recommendation algorithms.

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Recommendation Items</td><td>items</td><td>Array of <a href="#product">Product</a> objects</td><td>The products which have been recommended to the user on this page.</td></tr>
</table>

Example:

```javascript
window.universal_variable = {
	recommendation: {
		items: [
		{
			url: "http://www.example.com/product?=ABC123", 
			name: "ABC Trainers"
		},
		{
			url: "http://www.example.com/product?=DEF123", 
			name: "DEF Trainers"
		},
		{
			url: "http://www.example.com/product?=GHI123", 
			name: "GHI Trainers"
		}, ...]
	}
}
```

## EventList

The Eventlist object wraps an array of [Event](#event) objects.

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>EventList Items</td><td>items</td><td>Array of <a href="#event">Event</a> objects</td><td>The events which have happened since the last page view, or during the current page view.</td></tr>
</table>

## Event

The Event object identifies when something has just happened, either since the last page view, or during the current page view.

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Event Type</td><td>type</td><td>String</td><td>Label for the type of event that's taken place, e.g. 'conversion','signup'.</td></tr>
<tr><td>Event Time</td><td>time</td><td>String</td><td>String representation of the time at which this event occurred.</td></tr>
<tr><td>Event Cause</td><td>cause</td><td>String</td><td>Description of what caused this event, e.g. 'idle for 5 minutes'.</td></tr>
<tr><td>Event Effect</td><td>effect</td><td>String</td><td>Description for any user-facing activity that happens as a result of this event, e.g. 'popup shown'.</td></tr>
</table>

Example:

```javascript
window.universal_variable = {
	events: [{
		name: "newsletter_signup",
		cause: "checkout_popup"
	}]
}
 ```

## Journey

The Journey object is used as part of a travel-related [Product](#product), representing a single 'leg' of travel.

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Journey Type</td><td>type</td><td>String</td><td>Label for the type of journey, e.g. 'flight','train'.</td></tr>
<tr><td>Journey Name</td><td>name</td><td>String</td><td>Short description of this journey, e.g. 'Flight BA456 from JFK'.</td></tr>
<tr><td>Journey Code</td><td>code</td><td>String</td><td>Unique identifier for this journey, e.g. an Amadeus or Sabre code.</td></tr>
<tr><td>Journey Time</td><td>time</td><td>String</td><td><a href="http://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> representation of the date and/or time of travel.</td></tr>
<tr><td>Journey Adult Count</td><td>adults</td><td>Number</td><td>Number of adults travelling.</td></tr>
<tr><td>Journey Child Count</td><td>children</td><td>Number</td><td>Number of children travelling.</td></tr>
<tr><td>Journey Infant Count</td><td>infants</td><td>Number</td><td>Number of infants travelling.</td></tr>
</table>

Example:

```javascript
window.universal_variable = {
	product: {
		journeys: [{
		type: "flight",
		name: "Flight BA123 from London Heathrow",
		code: "FLIGHTCODE123",
		time: "2012-09-01 09:00",
		adults: 2,
		children: 2,
		infants: 0
		}]
	}
}
 ```

## Accommodation

 The Accommodation object is used as part of a travel-related [Product](#product).

Properties:

<table><tr><th>Property</th><th>JavaScript Key</th><th>Type</th><th>Description</th></tr>
<tr><td>Accommodation Type</td><td>type</td><td>String</td><td>Label for the type of accommodation, e.g. 'hotel'.</td></tr>
<tr><td>Accommodation Name</td><td>name</td><td>String</td><td>Short description, e.g. 'New York, Algonquin Hotel'.</td></tr>
<tr><td>Accommodation Code</td><td>code</td><td>String</td><td>Unique identifier, e.g. a reservation system code.</td></tr>
<tr><td>Accommodation Checkin Time</td><td>checkin_time</td><td>String</td><td><a href="http://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> representation of the date and/or time of checkin.</td></tr>
<tr><td>Accommodation Checkout Time</td><td>checkout_time</td><td>String</td><td><a href="http://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> representation of the date and/or time of checkout.</td></tr>
<tr><td>Accommodation Adult Count</td><td>adults</td><td>Number</td><td>Number of adults travelling.</td></tr>
<tr><td>Accommodation Child Count</td><td>children</td><td>Number</td><td>Number of children travelling.</td></tr>
<tr><td>Accommodation Infant Count</td><td>infants</td><td>Number</td><td>Number of infants travelling.</td></tr>
</table>

Example:

```javascript
window.universal_variable = {
	product: {
		accommodations: [{
		type: "hotel",
		name: "New York, Algonquin Hotel",
		code: "BOOKINGCODE123",
		checkin_time: "2012-09-01",
		checkout_time: "2012-09-08",
		adults: 2,
		children: 2,
		infants: 0
		}]
	}
}
 ```


