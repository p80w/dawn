# P80 Dawn Template 
Based on [DAWN](https://github.com/Shopify/dawn)

# Contributing Guidelines

## Branches and Naming Conventions

### Branches

_Every_ code update should have its own branch. No work should be done on the `main`/`master` branch.

Branches should have an easy-to-understand and _relevant_ name. When applicable, the branch for your work should mirror the title of the assigned task in Asana (or the project management tool it was created in).

#### For example:

Asana Task - `Component - Recently Viewed Products`

Branch name: `component-recently-viewed-products`

### File / Folder / Code Naming

File names & folder names should be dash-separated.

File naming should follow Shopify's naming system. Here are a few examples.

#### Example A: We are creating a new `section`
`sections/your-section-title.liquid` || `sections/faq-list.liquid`

`assets/section-your-section-title.css` || `assets/section-faq-list.css`

`assets/section-your-section-title.js` || `assets/section-faq-list.js`

#### Example B: We are creating a new `snippet`
`snippets/your-snippet-title.liquid` || `snippets/custom-pricing.liquid`

`assets/component-your-snippet-title.css` || `assets/component-custom-pricing.css`

`assets/component-your-snippet-title.js` || `assets/component-custom-pricing.js`

### Variable and function names should be `camelCase`

#### For example:

`p80.productHistory = function(config) {};`

`let productHistory;`

### JavaScript Conventions

When considering JavaScript code there are two acceptable methods of work. [Prototype](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) based components, and [Class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) based components. `Class` based components are what Shopify has typically adopted for their general DAWN components.

`Class` & `Prototype` based components are essentially identical - however `Class` based components are more recent, and support more tight-knight integration with the DOM. In the author's *opinion*, `Class` based components excel for simple, reuseable components whereas `Protoype` lends itself better to more complicated implementations. In the end, identical results are achievable with both.

For all applicable custom components/JavaScript using the `Prototype` format, we will be using the namespace `p80`. Each component should check if the namespace has already been initalized before declaring any other code.

#### For example:

```
if(typeof p80 === typeof undefined) {
    var p80 = {};
}
p80.productHistory = function(config) {};
```

When creating a `Class` based component you are still required to adhere to the above mentioned coding guidelines for things like variable naming, file & folder structure, and you should almost always avoid any global variable or function definitions outside of your main component definition whenever possible.

Here is an example of a `Class` based component from DAWN that is re-useable by simply copying the relevant DOM code

```
class QuantityInput extends HTMLElement {
  constructor() {
    super();
    this.input = this.querySelector('input');
    this.changeEvent = new Event('change', { bubbles: true })

    this.querySelectorAll('button').forEach(
      (button) => button.addEventListener('click', this.onButtonClick.bind(this))
    );
  }

  onButtonClick(event) {
    event.preventDefault();
    const previousValue = this.input.value;

    event.target.name === 'plus' ? this.input.stepUp() : this.input.stepDown();
    if (previousValue !== this.input.value) this.input.dispatchEvent(this.changeEvent);
  }
}

customElements.define('quantity-input', QuantityInput);
```

## Pull Requests

Each pull request should contain all information required for someone to understand the _purpose_ of your code change. This includes the original bug/request, and the steps taken in your code changes to rectify the bug or implement the request.

### Pull Request Title

The title of your pull request should reflect the name of the task, as well as the type of work being done.

`Create` for creating new code.

`Update` for adding functionality to existing code.

`Bugfix` for fixing bugs in existing code.

#### For example:

`Create - productHistory component`

OR

`Update - productHistory component history tracking`

OR

`Bugfix - productHistory component mobile styles`

Each pull request should also follow this general format:

```
## Description

// What does this code change do? Why was it required?

## Usage

// If applicable, how do we use this component? OR how does the client control this feature? (ie. Theme settings...etc. etc)

## Testing steps/scenarios
- [ ] _List all the testing tasks that applies to your fix and help peers to review your work._

## Demo links
_Please include a link to a store that includes preconfigured sections and settings to allow reviewers to easily test the features you are working on._

- [Store](url)
- [Editor](url)

**Checklist For Shopify Theme Development** 
Used for Shopify themes only. Can be removed if it does not apply.
- [ ] Followed [theme code principles](https://github.com/Shopify/dawn/blob/main/.github/CONTRIBUTING.md#theme-code-principles)
- [ ] Linted with [Theme Check](https://github.com/Shopify/theme-check)
- [ ] Tested on [mobile](https://shopify.dev/themes/store/requirements#mobile-browser-requirements)
- [ ] Tested on [multiple browsers](https://shopify.dev/themes/store/requirements#desktop-browser-requirements)
- [ ] Tested for [accessibility](https://shopify.dev/themes/best-practices/accessibility)

## UI

// If applicable, attach screenshots of the before/after state of your code change. Or, if this is a new component, attach screenshots of what the component should look like in its base implementation.

## Documentation

- [ ] Appropriate docs were updated (if necessary)

## Task Reference

// Please include a URL to where the task for this code change lives. The task will likely contain the information required to properly review your PR.

// [Component - Recently Viewed Products](https://app.asana.com/0/0/1201362516732595/f)
// [URL title](URL)
```

### Squash & Merge

With Squash & Merge enabled you will be asked to provide one commit message that will be pushed to master/main on merging. This saves us the clutter of having dozens of new commits listed on the main branch every time we merge a PR.

This commit message should be _informative_ and reflect the scope of the work being done.

#### For example:

`create productHistory component`

OR

`update logic bug in productHistory component`

OR

`update base styles in productHistory component`

Once your pull request has been merged, please delete the original branch (If it has not been automatically deleted).

## P80 Included Components

### Product Recommendations

This theme includes the [P80 Product Recommendations](https://github.com/p80w/shopify-code-snippets/tree/master/p80-product-recommendations) component extension.

This component works alongside the default product recommendations and will supercede the default Shopify recommendations when properly configured.

#### Setup

1) Create a storefront token and save it to p80.storefrontToken in `theme.liquid`
```
<script>
  if(typeof p80 === typeof undefined) {
    var p80 = {};
  }
  p80.storefrontToken = "aa8132c06be4596d4ef706b3cc37d6c9";
</script>
```
2) Create a product metafield for holding related products `p80.relatedProducts`
3) Populate the metafield on applicable products with a pipe `|` deliminated string of Product IDs e.g. `123456789|234567890|345678901`

After all above steps have been completed, the P80 Product Recommendations component should render in place of the default Shopify product recommendations.