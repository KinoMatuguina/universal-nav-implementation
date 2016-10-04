# ABS-CBN Universal Navigation
That 'thing' on top, also called the **Uninav**. The main goal of the script is to centralize the code, and unify the headers of ABS-CBN websites. The script currently has two versions.


## Setup
The script uses **Gulp** and **BrowserSync**. There are many references on the internet regarding **Gulp + BrowserSync** setup. Simply call `gulp` on the root of the Uninav directory to compile and watch the files.


## Version 1
Used for sites that only need to implement the **Uninav Button**, which shows the links of ABS-CBN websites.

**CSS:**
```html
<link rel="stylesheet" href="https://assets.abs-cbn.com/universalnav/uninav.css">
```

**JS:**
```html
<script src="https://assets.abs-cbn.com/universalnav/uninav.js"></script>
```

The main logic is to override websites based on their **domain name**. A couple of reasons for this approach:
- **We can target websites without heavily changing the markup**. All clients need to do is to put the Uninav CSS and JS on their templates.
- **The script was to be implemented on older sites** - sites which needed complicated approval on code changes both the frontend and the backend
- **Sites have different stylesheets**
- **Sites have different JS libraries**, specifically jQuery (including non-"$.fn.on" versions)
- If ever clients back out from its implementation, **they only have to remove two lines of code**
- Debugging is done only on the Uninav side

### Current sites of implementation as listed on the script: *(as of Jul 11, 2016)*

- http://www.abs-cbn.com
- http://www.abs-cbnnews.com
- http://abscbnmobile.com
- http://accounts.abs-cbn.com
- http://www.choosephilippines.com
- http://corporate.abs-cbn.com
- http://www.iwantv.com.ph
- http://dzmm.abs-cbnnews.com
- http://bmpm.abs-cbnnews.com
- http://internationalsales.abs-cbn.com
- http://marketportal.abs-cbn.com
- http://mmk.abs-cbn.com
- http://www.mor1019.com
- http://myxph.com
- http://push.abs-cbn.com
- http://starmagic.abs-cbn.com
- http://starcinema.abs-cbn.com
- http://starcinemamobile.abs-cbn.com
- http://starmusic.abs-cbn.com
- http://store.abs-cbn.com
- http://tvplus.abs-cbn.com

### Implementation

#### Step 1:
Create a new block on `domains` variable with the following base:
```javascript
'www.example.com': {
    togglerCreate: function() {
    },
    targetHeaderHeight: function() {
    },
    uninavClass: 'uninav-example'
}
```
* Let's assume the site is at `www.example.com`.
* `togglerCreate` is used to create the button/toggler. The function needs to return the toggler element.
* `targetHeaderHeight` is used to compute the height where the link will start sliding.
* `uninavClass` is a class appended to the body and used to customize the look of the elements through CSS.
* If the site is responsive, add `uninav-responsive` class (e.g. `uninavClass: 'uninav-example uninav-responsive'`)

#### Step 2:
Create a way to insert the universal nav toggler/button:
```javascript
togglerCreate: function() {
    // Using common toggler.template as the toggler's template (see src)
    $(toggler.template).appendTo('#someElement');
    // Always return .uninav-toggler!
    return $('.uninav-toggler');
}
```

#### Step 3:
Compute height from where the nav will start sliding:
```javascript
targetHeaderHeight: function() {
    return $('#siteHeader').outerHeight();
}
```

#### Step 4:
Create a new CSS block on `_main.scss` (in our case, this is `.uninav-example`):
```css
// www.example.com
.uninav-example {
    #someElement {
        padding: 20px;
    }
    .uninav-toggler {
        color: yellow;
    }
}
```


## Version 2
Used for sites that implement the whole universal header (folding header).

**CSS:**
```html
<link rel="stylesheet" href="https://assets.abs-cbn.com/universalnav/uninav-v2.css">
```

**JS:**
```html
<script src="https://assets.abs-cbn.com/universalnav/uninav-v2.js"></script>
```

This version relies on the markup being already present on the site of implementation. The script and the stylesheet then applies the proper styling and functionality.

### Current sites of implementation based on skins stylesheet: *(as of Jul 11, 2016)*

- Corporate
- Investor
- Entertainment
- Sports
- Lifestyle
- Television
- News
- Careers
- Licensing
- Chicken Pork Adobo
- TVplus
- ABS-CBN mobile
- StarCreatives
- Starmagic Workshop

### Base markup
```html
<header class="main uninav-header uninav-[layout] uninav-[skin-name]">
    <div class="header-fixed">
    </div>
</header>
```
* `[layout]` - The desired layout. Current layouts available:
    * `default`
    * `layout-2`
* `[skin-name]` - The site's skin name. Skins are located at `_header-skins.scss`
    * `corporate`
    * `investor`
    * `entertainment`
    * `sports`
    * `lifestyle`
    * `television`
    * `news`
    * `careers`
    * `licensing`
    * `cpa`
    * `tvplus`
    * `abscbnmobile`
    * `starcreatives`
    * `starmagic-workshop`

##### Sample:
```html
<header class="main uninav-header uninav-default uninav-corporate"></header>
```

#### Layout `default` markup

```html
<div class="header-bar">
    <button type="button" class="navbar-toggle collapsed header-nav-toggle" data-toggle="collapse" data-target="#header-collapse">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
    </button>
    <button id="uninav-toggle" type="button" class="navbar-toggle collapsed uninav-toggle pull-right">
        <span class="sr-only">Toggle Universal Navigation</span>
        <span class="uninav-icon uninav-icon-uninav"></span>
    </button>
    <div class="header-bar-inner">
        <h1 class="logo">
            <a href="index.html">
                <img src="img/logo-abs-cbn.png" alt="ABS-CBN"> <span>Site Name</span>
            </a>
        </h1>
        <ul class="header-social list-unstyled">
            <li class="search-container-lg"></li>
            <li><a href="#"><i class="uninav-icon uninav-icon-fb"></i> <span class="sr-only">facebook</span></a></li>
            <li><a href="#"><i class="uninav-icon uninav-icon-tw"></i> <span class="sr-only">twitter</span></a></li>
            <li><a href="#"><i class="uninav-icon uninav-icon-yt"></i> <span class="sr-only">youtube</span></a></li>
        </ul>
    </div>
</div>
<nav id="header-collapse" class="header-collapse collapse">
    <div class="header-nav">
        <div class="search-container">
            <form class="search-form" autocomplete="off">
                <input type="search" name="search" placeholder="Search">
                <input type="submit" value="Search" class="uninav-icon uninav-icon-search">
            </form>
        </div>
        <ul class="list-unstyled clearfix">
            <li class="active"><a href="#">Link 1</a></li>
            <li><a href="#">Link 2</a></li>
            <li>
                <a href="#">Link 3 with sublinks <span class="has-child-drop"></span></a>
                <ul>
                    <li><a href="#">Sublink 1</a></li>
                    <li><a href="#">Sublink 2</a></li>
                    <li><a href="#">Sublink 3</a></li>
                </ul>
            </li>
        </ul>
    </div>
    <div class="header-login">
        <ul class="list-unstyled">
            <li><a href="#"><i class="uninav-icon uninav-icon-login"></i> Log in</a></li>
            <li>
                <a href="#" class="clearfix header-profile">
                    <img src="http://placehold.it/20x20" alt=""> John Doe
                    <span class="pull-right text-underline">View Profile</span>
                </a>
            </li>
            <li><a href="#">Log Out</a></li>
        </ul>
    </div>
</nav>
```


#### Layout `layout-2` markup

```html
<div class="header-bar">
    <button type="button" class="navbar-toggle collapsed header-nav-toggle" data-toggle="collapse" data-target="#header-collapse">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
    </button>
    <button id="uninav-toggle" type="button" class="navbar-toggle collapsed uninav-toggle pull-right">
        <span class="sr-only">Toggle Universal Navigation</span>
        <span class="uninav-icon uninav-icon-uninav"></span>
    </button>
    <div class="header-bar-inner">
        <h1 class="logo">
            <a href="index.html">
                <img src="img/logo-abs-cbn.png" alt="ABS-CBN"> <span>[site-name]</span>
            </a>
        </h1>
        <nav id="header-collapse" class="header-collapse collapse">
            <div class="header-nav">
                <div class="search-container">
                    <form class="search-form" autocomplete="off">
                        <input type="search" name="search" placeholder="Search">
                        <input type="submit" value="Search" class="uninav-icon uninav-icon-search">
                    </form>
                </div>
                <ul class="list-unstyled clearfix">
                    <li class="active"><a href="#">Link 1</a></li>
                    <li><a href="#">Link 2</a></li>
                    <li>
                        <a href="#">Link 3 with sublinks <span class="has-child-drop"></span></a>
                        <ul>
                            <li><a href="#">Sublink 1</a></li>
                            <li><a href="#">Sublink 2</a></li>
                            <li><a href="#">Sublink 3</a></li>
                        </ul>
                    </li>
                </ul>
            </div>
            <div class="header-login">
                <ul class="list-unstyled">
                    <li><a href="#"><i class="uninav-icon uninav-icon-login"></i> Log in</a></li>
                    <li>
                        <a href="#" class="clearfix header-profile">
                            <img src="http://placehold.it/20x20" alt=""> John Doe
                            <span class="pull-right text-underline">View Profile</span>
                        </a>
                    </li>
                    <li><a href="#">Log Out</a></li>
                </ul>
            </div>
        </nav>
        <ul class="header-social list-unstyled">
            <li class="search-container-lg"></li>
            <li><a href="#"><i class="uninav-icon uninav-icon-fb"></i> <span class="sr-only">facebook</span></a></li>
            <li><a href="#"><i class="uninav-icon uninav-icon-tw"></i> <span class="sr-only">twitter</span></a></li>
            <li><a href="#"><i class="uninav-icon uninav-icon-yt"></i> <span class="sr-only">youtube</span></a></li>
        </ul>
    </div>
</div>
```


### Base Skin styling (based on Corporate skin)
```scss
header.uninav-[skin-name] {
    $bg-color: #252525;
    $text-color: #898989;
    $nav-color: #1e1e1e;
    $border-color: #898989;
    $inverse-color: #fff;
    &, .header-bar { background-color: $bg-color; }
    .header-bar {
        background-image: url(img/ring-black-md.png);
        @media (min-width: $screen-lg) {
            background-image: url(img/ring-black-lg.png);
        }
    }
    .uninav-toggle { background: darken($nav-color, 3%); }
    .header-nav-toggle[aria-expanded="true"] { background-color: $nav-color; }
    .icon-bar { background-color: $inverse-color; }
    .logo a { color: $inverse-color; }
    .header-collapse {
        background-color: $nav-color;
        a { color: $text-color; }
        a:hover, a:active { background-color: $bg-color; }
    }
    .header-nav {
        li.active > a { color: $inverse-color; }
        > ul {
            @media (min-width: $screen-lg) {
                > li:hover > a { background-color: $bg-color; }
            }
            ul {
                background-color: $bg-color;
                a:hover, a:active { background-color: $nav-color; }
            }
        }
        .has-child-drop {
            &:hover, &:active, &.active { background-color: $bg-color; }
            &:after { border-top-color: $border-color; }
        }
    }
    .search-container { input[type=text], input[type=search] { border-color: $inverse-color; color: $inverse-color; } }
    .search-container-lg .search-container:hover { input[type=text], input[type=search] { border-color: $inverse-color; color: $inverse-color } }
    .header-login a { border-top-color: $border-color; }
}
```

### Implementation and new skin creation

#### Step 1:
Start assembling the header markup. Choose your desired layout and paste inside the base markup. In this example, we're using the `default` layout with a skin of `newskin`. Customize based on requirements:
```html
<header class="main uninav-header uninav-default uninav-newskin">
    <div class="header-fixed">
        <div class="header-bar">
            <!-- more code... -->
        </div>
        <nav id="header-collapse" class="header-collapse collapse">
            <!-- more code... -->
        </nav>
    </div>
</header>
```

#### Step 2:
Create a new skin at `_header-skins.scss`. Use the template found on the **Base Skin styling** section. Customize based on requirements.


## Version 3

### Implementing UNINAV V3

### step 1
- Same implementation from uninav v2
- include jquery on the html [JQuery](https://code.jquery.com/)
- Include uninav under the JQuery
- Include the css below under head
```html
<link rel="stylesheet" type="text/css" href="http://assets.abs-cbn.com/universalnav/uninav-v3.css">
```
- Include this on the bottom of the body

```html
<script type="text/javascript" src="http://assets.abs-cbn.com/universalnav/uninav-v3.js"></script>    
```
### step 2

- include on you master css or if you have pages that not using sso set sso: `false`
```html
    <script>
        $('header').uninav({
            <!--settings goes here-->
        });
    </script>
```
### step 3
### Include mobile navigation

- include this on the page that you want to add mobile navigation

```php
    <div id="mobile-uninav"> </div>
```
 
### Use new icons

```html
    <!--old-->
    <i class="uninav-icon"></i>
    <!--new-->
    <i class="uninav-icon-new"></i>
```
### Use new search 
- include this before the facebook icon on header
```html
<li><a href="javascript:void(0);"><i class="uninav-icon-new uninav-icon-search"><span class="sr-only">search</span></i></a></li>
    
```

### Settings
```php
        type: "default", //required 
        uninav : true,  //required
        dataDomain: 'onemusic', //required
        mobileNav: true, //required
        oldsearch: true //for news
        sso: true //can use for special cases like login page , profile page 
```

- uninav, uninavAnimation, headerSticky, mobileNav is all - `boolean`
- dataDomain : `String` available data domain `onemusic, news, lifestyle, entertainment, sports, iwanttv, tfc, mysky, skyondemand`
- oldsearch : `boolean`
- uninavIconTarget : `target of the uninav button`
- customDashBoard : [{`title:` `string`, `url:` `string`}, {`title:` `string`, `url:` `string`} ... ]