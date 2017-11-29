##Magento1.x Property.js console.log() Not Work##

If you are writing any javascript in magento and try to debug using console.log, you may find that nothing happens in Chrome, but everything works as expected in Firefox.

The reason for this is that varien have included a blank console object to prevent any calls to the console throwing an error if a console isn’t defined. However they explicitly check for firebug, which means if your running chrome the built in console is replaced.

To fix this you need to edit the js/varien/js.js file and find the following line
`if (!(“console” in window) || !(“firebug” in console))`

Replace the line with the following
`if (!(“console” in window) || typeof console !== ‘object’)`

and you can start console logging in chrome

##Documents##
[https://edmondscommerce.github.io/magento/console-log-not-working-in-magento-solution.html](https://edmondscommerce.github.io/magento/console-log-not-working-in-magento-solution.html)