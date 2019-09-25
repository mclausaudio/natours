# Natours

## Fully responsive landing and marketing page built with Sass and advanced CSS techniques.

### By: [Michael Claus](https://www.michaelclaus.io)

---

Natours is a fully responsve landing page and marketing website built for a fictional company. My main goal for this project was to 'level up' my CSS and styling skills. To accomplish this, I decided to learn Sass and focus on a few key areas that include asymmetrical page layouts, utilizing transform effects to create engaging user interactions and keeping the code as 'DRY' as possible by creating reusable components. Below is a list of some of the technologies, features, and techniques I implemented in this project.

-   Sass & Semantic HTML
-   BEM naming conventions
-   Mixins
-   Variables
-   CSS prefixers and minifiers
-   Utilized the 'checkbox hack' to create a pop up navigation menu without JavaScript
-   CSS targets to launch modals.
-   Fully mobile and tablet responsive
-   Used multiple folders and files to keep the project scalable and organized

### Creating the navigation

To create the navigation, I utilized a CSS technique known as the checkbox hack. This involves hiding a input tag and styling it's associated label tag to look like a button. Clicking the label will toggle the inputs 'checked' state. I then used the :checked CSS pseudo element to target a background div that is underneath the label ONLY when the input tag has a state of checked. When checked, the background div is scaled to spread across the page and a list of links is centered. I also rotate the hamburger icon that I created to make an X.

HTML

```
<div class="navigation">
			<!-- Checkbox Hack -->
			<input
				type="checkbox"
				class="navigation__checkbox"
				id="navi-toggle"
			/>
			<label for="navi-toggle" class="navigation__button">
				<span class="navigation__icon">&nbsp;</span>
			</label>
			<div class="navigation__background">&nbsp;</div>
			<nav class="navigation__nav">
				<ul class="navigation__list">
					<li class="navigation__item">
						<a href="#" class="navigation__link">About Natours</a>
					</li>
					<li class="navigation__item">
						<a href="#" class="navigation__link">Your Benefits</a>
					</li>
					<li class="navigation__item">
						<a href="#" class="navigation__link">Popular Tours</a>
					</li>
					<li class="navigation__item">
						<a href="#" class="navigation__link">Stories</a>
					</li>
					<li class="navigation__item">
						<a href="#" class="navigation__link">Book Now</a>
					</li>
				</ul>
			</nav>
		</div>
```

CSS

```
.navigation {
	&__checkbox {
		display: none;
	}
	&__button {
		background-color: $color-white;
		height: 7rem;
		width: 7rem;
		position: fixed;
		top: 6rem;
		right: 6rem;
		border-radius: 50%;
		z-index: 2000;
		box-shadow: 0 1rem 3rem rgba($color-black, 0.5);
		text-align: center;
		cursor: pointer;
		@include respond(tab-port) {
			top: 4rem;
			right: 4rem;
		}
		@include respond(phone) {
			top: 2.5rem;
			right: 2.5rem;
		}
	}
	&__background {
		height: 6rem;
		width: 6rem;
		border-radius: 50%;
		position: fixed;
		top: 6.5rem;
		right: 6.5rem;
		background-image: radial-gradient(
			$color-primary-light,
			$color-primary-dark
		);
		z-index: 1000;
		transition: transform 0.3s ease-in;
		@include respond(tab-port) {
			top: 4.5rem;
			right: 4.5rem;
		}
		@include respond(phone) {
			top: 3rem;
			right: 3rem;
		}
	}
	&__nav {
		height: 100vh;
		position: fixed;
		left: 0;
		right: 0;
		z-index: 1500;
		//opacity 0 to make it invis, width 0 so links move out of way
		opacity: 0;
		width: 0;
		transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
	}
	&__list {
		@include absCenter;
		list-style: none;
		text-align: center;
		width: 100%;
	}
	&__item {
		margin: 1rem;
	}
	&__link {
		&:link,
		&:visited {
			display: inline-block;
			font-size: 3rem;
			font-weight: 300;
			padding: 1rem 2rem;
			color: $color-white;
			text-decoration: none;
			text-transform: uppercase;
			background-image: linear-gradient(
				120deg,
				transparent 0%,
				transparent 50%,
				$color-white 50%
			);
			// we start with a large background so only the transparent side of the gradient is visible.  then use background-position to shift it over to the other gradient side.
			background-size: 230%;
			transition: all 0.2s;
		}
		&:hover,
		&:active {
			background-position: 100%;
			color: $color-primary;
			transform: translateX(1rem);
		}
	}
	// checkbox hack
	//since clicking the label toggles checkbox inputs check state, we can select the checkbox when its checked and then select a sibling next to it to modify
	&__checkbox:checked ~ &__background {
		transform: scale(80);
	}
	&__checkbox:checked ~ &__nav {
		opacity: 1;
		width: 100%;
	}

	//ICON
	&__icon {
		position: relative;
		margin-top: 3.5rem;
		&,
		&::before,
		&::after {
			width: 3rem;
			height: 2px;
			background-color: $color-grey-dark-3;
			display: inline-block;
		}
		&::before,
		&::after {
			content: "";
			position: absolute;
			left: 0;
			transition: all 0.2s;
		}

		&::before {
			top: 0.8rem;
		}
		&::after {
			top: -0.8rem;
		}
	}

	&__button:hover &__icon::before {
		top: -1.1rem;
	}
	&__button:hover &__icon::after {
		top: 1.1rem;
	}
	//when checkbox is selected, select the adjacent button sibling and the icon within it
	&__checkbox:checked + &__button &__icon {
		background-color: transparent;
	}
	&__checkbox:checked + &__button &__icon::before {
		top: 0;
		transform: rotate(135deg);
	}
	&__checkbox:checked + &__button &__icon::after {
		top: 0;
		transform: rotate(-135deg);
	}
}
```

### Creating the rotating tour cards

The tour cards are one of my biggest components and are over 200 lines of sass code! To acomplish the 'flipping' interaction, I made two container divs and placed them on top of each other. I set both of these divs to have a 'backface-visibility' value of 'hidden'. This will back side of an element invisibile. The div which acts as the back of the card has an initial transform: rotateY() value of -180, which will make it invisible. When the user hovers, the div that acts as the front gets rotated 180 and the back side dive gets rotated back to 0. This gives the illusion that these cards are being flipped. The perspective CSS property is used to adjust the fluidity and 3d nature of the rotation.

```
.card {
	// FLIP FUNCTIONALITY
	perspective: 150rem;
	-moz-perspective: 150rem;
	position: relative;
	height: 52rem;

	&__side {
		transition: all 0.8s ease;
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		//if set to hidden, backface-visibility hides the back part of an element
		backface-visibility: hidden;
		border-radius: 3px;
		//set overflow to hidden, because child background image "overflows" parent, and makes top look square, not like it has border-radius
		overflow: hidden;
		box-shadow: 0 1.5rem 4rem rgba($color-black, 0.15);

		&--front {
			background-color: $color-white;
		}
		&--back {
			transform: rotateY(180deg);

			&-1 {
				background-image: linear-gradient(
					to right bottom,
					$color-secondary-light,
					$color-secondary-dark
				);
			}
			&-2 {
				background-image: linear-gradient(
					to right bottom,
					$color-primary-light,
					$color-primary-dark
				);
			}
			&-3 {
				background-image: linear-gradient(
					to right bottom,
					$color-tertiary-light,
					$color-tertiary-dark
				);
			}
		}
	}

	//card rotation
	&:hover &__side--front {
		transform: rotateY(-180deg);
	}
	&:hover &__side--back {
		transform: rotateY(0deg);
	}
```
