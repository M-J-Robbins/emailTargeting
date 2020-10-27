# This repo is no longer maintained and some of the content may be out of date.
# Instead you can find all this information and more at https://howtotarget.email/

------------

# Email client CSS targeting 

This is a list of tricks used to target individual email clients.  These are hacky and likely to change, use it as a list of possible suggestions not a set of reliable rules.

## Airmail

`.bloop_container .foo`

## Android

`_:-webkit-full-screen, :root .foo` and `@media screen and (pointer) { .foo}`

### - 2.3

`@media screen and (min-width:0\0){ .foo }`
This broken media query will also show in Outlook 00-03 so may need to add an IE conditional comment.

### - 4.2

`_:-webkit-full-screen, :root .foo`
This won't render on iOS devices but will render on desktop so may need a max-device-width media query.

### - 4.4

`body[style] div[style="margin: 16px 0; font-size: 80%"]`

## AOL

***AOL now uses Yahoo rendering but the previous hack was ***

`.★:not(#★)`
AOL respects a unicode name for a class but not for an ID, it will remove the ID completely from the element.

`.body`
AOL also removes class from the body tag so you can use `.body` to avoid AOL

## Apple Mail

### - 10 - current

`.Singleton .foo`

### - 8

`_:-webkit-full-screen, _::-webkit-full-page-media, _:future, :root .foo`

## Edison

`#edo-container .foo`
Will cover both iOS and Android 

### - Android

`.edo-email-view .foo`

### - iOS

`.edo .foo`

## Freenet

* `#msgBody .foo`
* The head and body structure of the email is removed, making content in the head, siblings of that in the body, so we can target this with; `meta ~ * .foo` or `title ~ * .foo` Applies to Thunderbird, Freenet(title only), Orange.fr, Samsung

## Gmail/Inbox

`u + .body .foo`
Gmail changes the `doctype` to `<u></u>` this is placed adjacent to a div that inherits class and id from the body tag.

### - Gmail app on android

`div > u + .body .foo`
This doesn't cover Inbox, only Gmail and only Android.

## GMX, web.de

*Work in progress*. Any class or id name prefixed with an unsupported character will kill all the styles following it. These include `&`, `1`, `★`, `~`.

### - GMX Samsung7

`body[style*="overflow-wrap:break-word"]`

> Small possibility this will cause conflict

## iCloud

*Work in progress. *Links have `rel="noreferrer"` added. Strips a number of attributes including `role` `aria` `data-`

## iOS

`@supports (-webkit-overflow-scrolling:touch) and (color:#ffff) { .foo }`
Works for 10+

## Newton

Mobile - `.cm_mail .foo`
Desktop - `#cm_mail_smart_body .foo`

## Nine

`.body > div > div > .wrapper .foo`
Nine inserts a couple of Div's between the body and the wrapper. Same hack also targets Samsung and possibly others.

## Notes

`.unused.foo`
Only works on Notes 8, it will strip the unrecognised class and render the code behind.

## Open-Xchange (Comcast, Libero, Virgin Media)

Open-Xchange powers a number fo different email clients, including; Comcast, Libero, 1&1 MailXchange, Network Solutions Secure Mail, Namecheap Email Hosting, Mailbox.org, 123-reg Email, acens Correo Professional, Home.pl Cloud Email Xchange, Virgin Media Mail, Ziggo Mail. They prefix class and ID names with `ox-` so we can target them with this
`.foo[class^="ox-"]`
Link to [sanitizer docs](https://gitlab.open-xchange.com/frontend/core/blob/02be9f336a69556a1016e1ae266dba823d208511/ui/apps/io.ox/mail/sanitizer.js#L23) Which appears to be built on top of [DOMPurify](https://github.com/cure53/DOMPurify) [List of email clients on wikipedia](https://en.wikipedia.org/wiki/Open-Xchange#cite_ref-3)

## Orange

The head and body structure of the email is removed, making content in the head, siblings of that in the body, so we can target this with; `meta ~ * .foo` or `title ~ * .foo` Applies to Thunderbird, Freenet(title only), Orange.fr, Samsung

## Outlook

### - Desktop windows

`.&` This is an invalid selector but works in MSO outlook.

To show content:
`<!--[if mso]> <![endif]-->` 
`<!--[if true]> <![endif]-->` 
`<!--[if vml]> <![endif]-->`
To hide content
`<!--[if mso]><!--> <!--<![endif]-->` 
`<!--[if false]> <![endif]-->`
Other conditional
`<!--[if IE]> <![endif]-->` 
`<!--[if WindowsEdition]> <![endif]-->`

### - Desktop mac

`_:-webkit-full-screen, _::-webkit-full-page-media, _:future, :root .body:not(.Singleton) .foo`
The stuff before `.body` will target webkit desktop apps on mac (Applemail and Outlook Mac), then we add the `:not(.Singleton) `to remove Applemail.

This also targets Spark desktop app.

To be able to inspect element in Outlook Mac, and some other desktop Mac email clients, open terminal and paste in this line 

```
defaults write NSGlobalDomain WebKitDeveloperExtras -bool true
```

### - Webmail

`[owa].foo`
`x:default .foo`

Adding `\0/` to the end of a style e.g.  `background:red\0/;` will also render in Apps and Outlook 2000-2003

### - iOS app

`body[data-outlook-cycle]` this attribute is added to both iOS and Android apps, also renders in webmail if you drop the `body` and just use the attribute.

There appears to be some inconsistencies in app versions
`body[style*="touch-action: manipulation;"] .foo`
Sometime code in a linked style sheet will render.

### - Android app

`body[data-outlook-cycle]` this attribute is added to both iOS and Android apps, also renders in webmail if you drop the `body` and just use the attribute.

`[data-outlook-cycle*="INSERT_STYLES"]` will target only non Microsoft email addresses `@hotmail, @live, @outlook etc.` on iOS.

## Postbox

`.moz-text-html .foo`
Same hack as thunderbird, if rendering is different we can add to this.

## Samsung

### - S4

`#secdiv`

### - S5 to S9 - (double check s9)

* `#MessageViewBody`
* `.body > div > div > .wrapper .foo` Samsung inserts a couple of Div's between the body and the wrapper. Same hack as Nine.
* The head and body structure of the email is removed, making content in the head, siblings of that in the body, so we can target this with; `meta ~ * .foo` or `title ~ * .foo` Applies to Thunderbird, Freenet(title only), Orange.fr, Samsung

## Spark

### - desktop

`_:-webkit-full-screen, _::-webkit-full-page-media, _:future, :root .body:not(.Singleton)`
Same target as Outlook mac

## Thunderbird

* `.moz-text-html .foo`
* This class is placed on a div inserted between the body and the wrapper so you can also do something like `body > div > .wrapper .foo`
* The head and body structure of the email is removed, making content in the head, siblings of that in the body, so we can target this with; `meta ~ * .foo` or `title ~ * .foo` Applies to Thunderbird, Freenet(title only), Orange.fr, Samsung

## T-online.de

`<!--[if false]> T-Online <![endif]-->` or `<!--[if !true]> T-Online <![endif]-->`
Supports ANY conditional comment but needs to be something valid so IE Outlooks don't show it. 

## Windows Phone, Surface

`_:-ms-input-placeholder, :root .foo` `_:-ms-fullscreen, :root .foo`
These are from very old test, possible overlaps with Windows Phone.

## Yahoo, AOL

`.& .foo{}`
Yahoo and AOL will replace the `.&` with their wrapping id name.  It can also be used to target element selectors `.&h1{}` and the wrapping div itself `.&{}` 

`@media screen yahoo{ .foo }`
Yahoo & AOL remove invalid media query selectors so will render the above as `@media screen {}`. Yahoo doesn't support `max-device-width` so makes it tricky to split mobile but we can use `max-width`

We can also separate yahoo from AOL using `id="★foo"` which AOL will strip out but yahoo will leave. AOL also strips `<title>` and empty `<style>` elements

### - iOS app

(tbc)

### - Android app

Android will ignore styles in the head. We can change this by adding an additional head before the real head. `<head></head><head> - styles etc. - </head>`
