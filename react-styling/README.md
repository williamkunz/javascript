# React CSS Style Guide

* [styled jsx documentation](https://github.com/zeit/styled-jsx#styled-jsx) **preferred*
* [styled components](https://github.com/styled-components/styled-components) **supported but not recommended for maintainability*
* [glamor](https://github.com/threepointone/glamor#glamor) **supported but not recommended*


## Table of Contents

1. [Example](#example)
1. [Ordering](#ordering)
1. [Theming](#theming)
1. [Variables](#variables)

## Example

  - Typical `Heading` Component example
  
  ```jsx
  import React, { Component } from 'react'
  import PropTypes from 'prop-types'
  
  // style vars
  import { fontBlack, headingBackgroundColor } from 'styles/colors'
  
  class Heading extends Component {
  	  constructor(props) {
  	  	super(props)
  	  	this.displayName = 'Heading'
  	  }
  	  
  	  render() {
  	  	const { subText, title } = this.props
  	  	
  	  	return (
  	  		<header className="heading" role="heading">
  	  			<h1>{title}</h1>
  	  			<p>{subText}</p>
  	  			
  	  			<style jsx>{`
  	  				.heading {
  	  					background-color: ${ headingColor };
  	  					margin: 1rem auto;
  	  					max-width: 40rem;
  	  				}
  	  				
  	  				h1 {
  	  					color: ${ fontBlack };
  	  					font-size: 2rem;
  	  					font-weight: 500;
  	  				}
  	  				
  	  				p {
  	  					font-size: 1.4rem;
  	  					text-align: center;
  	  				}
  	  				
  	  				@media only screen and (min-width: 70rem) {
  	  					.heading {
	  	  					margin-bottom: 2rem;
	  	  					margin-top: 2rem;
	  	  					max-width: 70rem;
	  	  				}
  	  				}
  	  			`}</style>
  	  		</header>
  	  	)
  	  }
  }
  
	Heading.propTypes = {
		subText: PropTypes.string.isRequired,
		title: PropTypes.string.isRequired,
	}
  
  export default Heading
  
  ```


## Ordering

  - `style` tag always nested immediately within root element, at bottom.

    > Why? Styles can only be applied within the tag's immediate parent, siblings and children

    ```jsx
    // bad
    const MyComponent = () => {
      return (
        <div className="my-component">
          <p>Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.</p>
          
          <figure>
          	<img src="/myimg.png" alt="my img" />
          	
          	<style jsx>{`
          		// not immediate parent, unreachable
          		.my-component {
          			color: #000;
          		}
          	`}</style>
          </figure>
        </div>
      )
    }

    export default MyComponent


    // good
    const MyComponent = () => {
      return (
        <div className="my-component">
          <p>Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.</p>
          
          <figure>
          	<img src="/myimg.png" alt="my img" />
          </figure>
          
          <style jsx>{`
	      		// immediate parent
	      		.my-component {
	      			color: #000;
	      		}
	      	`}</style>
        </div>
      )
    }

    export default MyComponent
    ```
    
  - Within each style selector, order properties in alphabetical order
	
	> Why? Improves readability
	
	```jsx
	// bad
	const MyComponent = () => {
      return (
        <div className="my-component">
          <p>Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.</p>
          
          <style jsx>{`
	      		.my-component {
	      			color: #000;
	      			font-size: 1.5rem;
	      			left: 0;
	      			position: absolute;
	      			text-align: center;
	      			top: 0;
	      			z-index: 2;
	      		}
	      	`}</style>
        </div>
      )
    }

    export default MyComponent
    
    // good
    const MyComponent = () => {
      return (
        <div className="my-component">
          <p>Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.</p>
          
          <style jsx>{`
	      		.my-component {
	      			color: #000;
	      			font-size: 1.5rem;
	      			left: 0;
	      			position: absolute;
	      			text-align: center;
	      			top: 0;
	      			z-index: 2;
	      		}
	      	`}</style>
        </div>
      )
    }

    export default MyComponent
	```
  
## Theming

- `styled-jsx` only supports static variables imports 
	 
> Why? As to not rely on runtime rending with styles. Render styles once, not every re-render

```jsx
	// bad
    const MyComponent = ({ isDark }) => {
      return (
        <div className="my-component">
          <p>Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.</p>

          <style jsx>{`
	      		.my-component {
	      			color: ${ isDark ? '#000' : '#fff' };
	      		}
	      	`}</style>
        </div>
      )
    }
    
    // good
    const MyComponent = ({ isDark }) => {
      return (
        <div className={isDark ? 'my-component dark' : 'my-component'}>
          <p>Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.</p>

          <style jsx>{`
	      		.my-component {
	      			color: #fff;
	      		}
	      		
	      		.my-component.dark {
	      			color: #000;
	      		}
	      	`}</style>
        </div>
      )
    }
```

## Variables

- Style variables should be named descriptive with property involved into its name

> Why? To disallow the chance of duplicate variable namespacing.

```jsx
// bad
import { black } from 'styles/colors'

const MyComponent = () => {
  return (
    <div className="my-component">
      <p>Never doubt that a small group of thoughtful, committed citizens can
      change the world. Indeed, it’s the only thing that ever has.</p>

      <style jsx>{`
      		.my-component {
      			color: ${ black };
      		}
      	`}</style>
    </div>
  )
}

// good
import { fontColorBlack } from 'styles/colors'

const MyComponent = () => {
  return (
    <div className="my-component">
      <p>Never doubt that a small group of thoughtful, committed citizens can
      change the world. Indeed, it’s the only thing that ever has.</p>

      <style jsx>{`
      		.my-component {
      			color: ${ fontColorBlack };
      		}
      	`}</style>
    </div>
  )
}
```

- Style variables should be able to be exported separately from each other.

> Why? So each component does not have to import the weight of a large theming object.

```js
// bad

// file styles -> colors.js
export default {
	fontColorBlack: '#000',
	fontColorWhite: '#fff',
}

// file styles -> spacing.js
export default {
	gutterSmall: '1.6rem',
	gutterLarge: '3.2rem',
}


// good

// file styles -> colors.js
export const fontColorBlack = '#000'
export const fontColorWhite = '#fff'

// file styles -> spacing.js
export const gutterSmall = '1.6rem'
export const gutterLarge = '3.2rem'
```


