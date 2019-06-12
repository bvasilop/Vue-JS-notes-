# Vue-concepts

## V-bind

### Attribute binding

-   `v-bind:src="image"` think of it like `{{image}}` dynamically binds an attribute to an expression
-   shortcut for `v-bind:src="image"` is `:src="image"`
-   other abbreviated examples of **`v:bind`** are
    `:alt"description"` `:href="url"` `:title="toolTip` `:class="isActive"` , `:disabled="isDisabled"` `:style:"isStyled"`

```javascript
<div id = "app">
    <div class = "product">
        <div class ="product-image">
        <img v-bind:src="image>
        </div>

        <div class = "product-info">
            <h1>{{ title }}</h1>
            <p v-if="inStock">In Stock</p>
            <p v-else>Out of Stock</p>
            <p>{{ sale }}</p>
            <p> User is premium: {{ premium }}</p>
            <p> Shipping: {{ shipping }}</p>
            <ul>
                <li v-for="detail in details">{{ detail }}</li>
            </ul>
        </div>
    </div>
</div>
```

```javascript
var app = new Vue({
    el: '#app',
    data: {
        brand: 'Vue Mastery'
        product: 'Socks',
        selectedVariant:0,
        inStock: true,
        details: ['80% cotton', '20% polyester', 'Gender-neutral'],
        variants: [
            {
                variantId: 2234,
                variantColor:"green
            },
            {
                variantId: 2235,
                variantColor:"blue"
            },
        ],
        cart: 0,
        onSale: true,
        color: 'red'
        fontSize: '13px",
        styleObject: {
            color: 'red,
            fontSize:'13px'
        },
    },

    methods: {
        addToCart() {
            this.$emit('add-to-cart, this.variant[this.selectedVariant].variantId)
        },
        updateProduct(index) {
            this.selectedVariant= index
            console.log(index)
        },
        updateCart(id) {
            this.cart.push(id)
        },
        addReview(productReview) {
            this.reviews.push(productReview)
        }
    },
    computed: {
        title() {
            return this.brand + ' ' + this.product
        },
        image() {
                return this.variant[this.selectedVariant].variantImage
        },
        inStock() {
                return this.variants[this.selectedVariant].variantQuantity
        },
        sale() {
            if(this.onSale) {
                return this.brand + ' ' + this.product + ' are not on sale!'
            }
        },
        shipping() {
            if (this.premium) {
                return "Free"
            }
            return 2.99
        },
    }
});
```

### Dynamically binding images

-   ```javascript
      <img :src="image" />
    ```

### Conditional rendering

-   ```javascript
      <p v-if="inStock>In Stock</p>
      <p v-else>Out of stock</p>
      <p v-else-if="inventory <=10 && inventory > 0">Almost sold out!</p>
    ```

-   toggling visibility off and on

```javascript
<p v-show="inStock">In stock</p>
```

### List rendering

-   loops over an array

```javascript
<ul>
    <li v-for="detail in details">{{ detail }}</li>
</ul>

<div v-for="(variant, index) in variants"
    :key="variant.varaintId"
    class="color-box"
    :style="{ backgroundColor:variant.variantColor }"
    @mouseover="updateProduct(index)">{{ variant.variantColor :key="variant.variantID }}>
</div>
```

-   make sure to use `:key=""` to bind key to values.

## Event handling

-   `<button v-on:click="cart +=1">Add to Cart</button>`
-   using methods and shorthand - `<button @click="addToCart">Add to Cart</button>`
-   `<form @submit="addToCart">Add to Cart</form>`
-   `<div @mouseover="updateProduct">Color</div>`
-   `<input @keyup.enter="send">`

## Class and Style Binding

### Style Binding

-   `class="color-box"`
    `style="{ backgroundColor:variant.variantColor }"`

-   `<p :style="{fontSize: fontSize }"></p>`
-   `<p :style="{color: color }"></p>`
-   can use camelCase **fontSize** or kabob case **"font-size"** as long as it's in quotes
-   can be cleaner to bind to an object instead styleObject

### Class binding

-   `<button v-on:click="addToCart" :disabled="!inStock" :class="{ disabledButton: !inStock }">Add to Cart></button>`

## Computed properties

-   ````computed: {
          title() {
              return this.brand + ' ' + this.product
          }```

    ````

-   `<h1>{{ title }}</h1>`
-   **computed properties** are cached meaning the result is saved until its dependencies (this.brand - this.product) are changed.
-   it's more efficient to use a computed property rather than a method because it can be expensive re-running every time you need access. That's why caching is helpful.

## Components

### Props

-   a custom attribute for passing data into our components

-   Example: `props: [message]`

`<product message="hello"></product>`

-   another example: `<product :premium="premium"></product>`

#### Prop Validation

```
props: {
    message: {
        type: String,
        required: true,
        default: "Hi"
    },
    premium: {
        type: Boolean,
        required: true
    }
},
```

## Communicating Events

-   Say we have a cart that we want to use in other components, how would we implement?
-   How do we let parent components know events have happened?
-   How do we increment from inside our products component?
    1. We can let our parent component know I button is clicked by emitting an event

`<product :premium="premium" @add-to-cart="updateCart"></product>`

`<product-review @review-submitted="addReview"></product-review>`

```
Vue.component('product, {
    addToCart() {
        this.$emit('add-to-cart)
    }
})
```

## Two-way data binding

```javascript
Vue.component('product-review', {
    template: `
        <form class="review-form" @submit.prevent="onSubmit"> // prevents default behavior. Page won't refresh when we submit form
        <p>
            <label for="name">Name:</label>
            <input id="name" v-model="name">
        </p>
        <p>
            <label for="review">Review:</label>
            <textarea id="review" v-model="review"></textarea>
        </p>
        <p>
            <label for="rating">Rating:</label>
            <select id="rating v-model.number="rating">
            <option>5</option>
            <option>4</option>
            <option>3</option>
            <option>2</option>
            <option>1</option>
            </select>
        </p>
    `,
    data() {
        return {
            name: null,
            review: null,
            rating: null,
        };
    },
    methods: {
        onSubmit() {
            let productReview = {
                name: this.name,
                review: this.review,
                rating: this.rating
            };
            this.$emit('review-submitted', productReview)
            this.name= null,
            this.review = null,
            this rating = null
        },
    },
});
```
