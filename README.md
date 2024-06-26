# ngx-Matomo 

[![Build Status](https://travis-ci.com/Arnaud73/ngx-matomo.svg?branch=master)](https://travis-ci.com/Arnaud73/ngx-matomo)
[![NPM version](https://img.shields.io/npm/v/ngx-matomo.svg)](https://www.npmjs.com/package/ngx-matomo)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/b650cf6a9d3d4ab393af8d29d63fc8cc)](https://www.codacy.com/app/Arnaud73/ngx-matomo?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=Arnaud73/ngx-matomo&amp;utm_campaign=Badge_Grade)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg)](https://conventionalcommits.org)
[![MIT license](http://img.shields.io/badge/license-MIT-brightgreen.svg)](http://opensource.org/licenses/MIT)
[![dependencies Status](https://david-dm.org/Arnaud73/ngx-matomo/status.svg)](https://david-dm.org/Arnaud73/ngx-matomo)
[![devDependencies Status](https://david-dm.org/Arnaud73/ngx-matomo/dev-status.svg)](https://david-dm.org/Arnaud73/ngx-matomo?type=dev)
[![peerDependencies Status](https://david-dm.org/Arnaud73/ngx-matomo/peer-status.svg)](https://david-dm.org/Arnaud73/ngx-matomo?type=peer)

Wrapper for Matomo (aka. Piwik) analytics tracker for applications based on Angular 8, 9, 10, 11 & v12.

This is a fork of: [Arnaud73/ngx-matomo](https://github.com/Arnaud73/ngx-matomo)

## Installation

Use `npm` or `yarn` to add the module to your current project:
```npm install --save ngx-matomo```

## Adding Matomo into to your Angular application

You can add Matomo either via script tag or using the MatomoInjector in your root component.

### Initialize Matomo via Script Tag

To illustrate the set up, here's the code to inject into your header to initialize Matomo in your application. Matomo's [site](https://developer.matomo.org/guides/tracking-javascript-guide) has the detailed documentation on how to set up communication between Matomo and your application.
Make sure you replace the MATOMO_URL with your Matomo server. You can remove all the _paq methods in this script and set them up in your Angular 5+ application.

```html
<!-- Matomo -->
<script type="text/javascript">
  var _paq = _paq || [];
  _paq.push(['trackPageView']);
  _paq.push(['enableLinkTracking']);
  (function() {
    var u="//{$MATOMO_URL}/";
    _paq.push(['setTrackerUrl', u+'matomo.php']);
    _paq.push(['setSiteId', {$IDSITE}]);
    var d=document, g=d.createElement('script'), s=d.getElementsByTagName('script')[0];
    g.type='text/javascript'; g.async=true; g.defer=true; g.src=u+'matomo.js'; s.parentNode.insertBefore(g,s);
  })();
</script>
<!-- End Matomo Code -->
```

### Initialize Matomo via root component and MatomoInjector service

To enable Matomo via your root component you can now inject the MatomoInjector in your root component.

```ts
import { Component } from '@angular/core';
import { MatomoInjector } from 'ngx-matomo';

@Component({
  selector: 'app',
  template: `<router-outlet></router-outlet>`
})
export class AppComponent {
  constructor(
    private matomoInjector: MatomoInjector
  ) {
    this.matomoInjector.init('YOUR_MATOMO_URL', YOUR_SITE_ID);
  }
}
```

## Include it in your application

Bootrapping this application is easy. Import ```MatomoModule``` into your root ```NgModule```.

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { MatomoModule } from 'ngx-matomo';

import { AppComponent } from './app.component';

@NgModule({
  imports: [
    BrowserModule,
    MatomoModule
  ],
  declarations: [AppComponent],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Once that's done you can import ```MatomoTracker``` into any component in your application.

```ts
// component
import { Component } from '@angular/core';
import { MatomoTracker } from 'ngx-matomo';

@Component({
  selector: 'app',
  template: `<router-outlet></router-outlet>`
})
export class AppComponent {
  constructor(
    private matomoTracker: MatomoTracker
  ) { }

  ngOnInit() {
    this.matomoTracker.setUserId('UserId');
    this.matomoTracker.setDocumentTitle('ngx-Matomo Test');
  }
}
```

## Tracking events

For now tracking events and actions is manual and is not injected into the html. 

```html
<button (click)="whatHappensOnClick(1)"></button>
```

```ts
// component
import { Component } from '@angular/core';
import { MatomoTracker } from 'ngx-matomo';

@Component({
  selector: 'app',
  templateUrl: './myButton.html'
})
export class MyComponent {
  constructor(
    private matomoTracker: MatomoTracker
  ) { }

  whatHappensOnClick(someVal){
    /*
     * some code...
     */
    this.matomoTracker.trackEvent('category', 'action', 'name', someVal);
  }
}
```

## Original Source

This module is lousily inspired from [Angular2Piwik](https://github.com/awronka/Angular2Piwik), which was also inspired from [Angulartics 2](https://github.com/angulartics/angulartics2).

## License

[MIT](LICENSE)

## See also

Matomo's [site](https://developer.matomo.org/) has the detailed documentation on how to use Matomo and integrate it in an application.
See also:
-   [Single-Page Application Tracking](https://developer.matomo.org/guides/spa-tracking)
-   [JavaScript Tracking Client](https://developer.matomo.org/guides/tracking-javascript-guide)
-   [JavaScript Tracking Client](https://developer.matomo.org/api-reference/tracking-javascript)
-   [Tracking HTTP API](https://developer.matomo.org/api-reference/tracking-api)
