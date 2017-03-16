# Quick and Dirty Draw Menu for React

## SASS Mixin

The draw mixin :cocktail:

```
/*=============================================>>>>>
= Quick and Dirty Draw Mixix =
===============================================>>>>>*/
@mixin draw($background: #000, $draw_offset: 6px, $draw_width: 20px, $draw_border_thickness: 2px, $top_offset: 30px) {
  .draw{
    display: block;
    position: absolute;
    top: 0;
    right: 0;
    font-size: 0;
    text-decoration: none;
    opacity: 1;
    transition: opacity 0.3s 0.3s;
    width: $draw_width;
    height: $draw_border_thickness;
    background: $textcolor;
  }
  &:before, &:after{
    content: '';
    position: absolute;
    top: -$draw_offset;
    right: 0;
    height: $draw_border_thickness;
    width: $draw_width;
    background: $background;
    transition: top 0.3s 0s, transform 0.3s 0s;
  }
  &:after{
    top: $draw_offset;
  }
}

@mixin draw-open($background: #000) {
  .draw{
    background: $background;
    opacity: 0;
    transition: opacity 0.3s;
  }
  &:before{
    transform: rotate(45deg);
    top: 0px;
    transition: top 0.3s 0.3s, transform 0.3s 0.3s;
  }
  &:after{
    transform: rotate(-45deg);
    top: 0px;
    transition: top 0.3s 0.3s, transform 0.3s 0.3s;
  }
}
```

## SASS Implementation

Practical use inside a partial

```
.nav_banner{
    // Some styles and shit like 'position:relative;'
    .toggle-nav{
      position: absolute;
      top: 30px;
      right: 20px;
      cursor: pointer;
      padding: 10px 0;
      @include draw(#666);
    }
    &.open{
      .toggle-nav{
        z-index: 9;
        @include draw-open(#666);
      }
    }
}
```

## React land
```
import React, { Component } from 'react';

export default class Navigation extends Component {
  constructor(props) {
    super(props);
    this.state = {
      navOpen: false
    };
  }
  // Handles nav toggle
  toggleNav(event) {
    event.preventDefault();
    this.setState({navOpen: !this.state.navOpen});
  }
  render() {
    const { navOpen } = this.state;
    return (
      <nav className={navOpen ? "nav_banner open" : "nav_banner"} role="navigation">
        <div to="#" onClick={(event) => this.toggleNav(event)} className="toggle-nav">
          <span className="draw">{navOpen ? 'Close' : 'Open'}</span>
        </div>
      </nav>
    );
  }
}
```
