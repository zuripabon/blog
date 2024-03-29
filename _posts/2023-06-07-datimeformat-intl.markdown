---
layout: post
title:  "Adoption of Intl.DateTimeFormat"
category: Tech
author: Zuri Pabón
---

Handling date formatting can be a challenge, often leading to inconsistencies and compatibility issues and requiring external libs. 

A current common approach in many organizations is to pass literal specific date time formats in functions whenever it is required to be displayed. This lead us to having different formats across apps, emails, native apps, and even within the same app. 

In order to address this, a proposal would be the adoption of `Intl.DateTimeFormat` which relies on the locale format eliminating the need to manually specify date formats and reducing the risk of inconsistencies.

Here some current examples of different formats we had in different projects at my current job:

* YYYY-MM-DD:

* DD MMM YYYY

* MM/dd/yyyy

* MMMM YYYY

* DD-MM-YYYY

Next we dive into the advantages/disadvantages I have discovered when using `Intl.DateTimeFormat`

## The advantages

Adopting `Intl.DateTimeFormat` we could get rid of all those different datetime formats and delegate that responsability to the presentation/rendering layer. Some of the advantages we could get from it:

* Consistent Date Formatting: Adopting `Intl.DateTimeFormat` ensures consistent date formatting across different components and modules. It prevents the display of dates in inconsistent or confusing formats, providing a unified user experience.

* Localization Support: `Intl.DateTimeFormat` seamlessly integrates with the Internationalization API, enabling easy localization of date formats based on the user's preferred language and region. This promotes a user-friendly experience by displaying dates in the appropriate format for different locales.

* Simplified Implementation: With `Intl.DateTimeFormat`, developers no longer need to write custom date formatting logic or rely on external libraries. This simplifies the codebase, reduces dependencies, and makes the code more maintainable and easier to understand.

* Future Compatibility: `Intl.DateTimeFormat` is widely supported in modern browsers, ensuring compatibility with a majority of user environments. Relying on a native JavaScript API like `Intl.DateTimeFormat` ensures future compatibility and reduces the risk of breaking changes or deprecated libraries.


## The challenges

There are important challenges to look at when adoptin this approach. Consider them carefully before going forward as they might cause a negative impact on your organization. Make sure your team agree on this drawbacks and have some fallback/backup solutions as you go with it

* Browser Compatibility: While `Intl.DateTimeFormat` is well-supported, some older or less commonly used browsers may lack full support. To address this, fallback options or polyfills can be used to ensure consistent behavior across different browsers.

* Integration with current stack. Format-message and similar libraries might require a progressive migration

* Server-side Rendering Challenges: When implementing server-side rendering, using `Intl.DateTimeFormat` can be more complex. Server environments may lack full support for the Internationalization API, requiring additional considerations and potential workarounds to ensure consistent date formatting across server-rendered and client-rendered content.

        For example, `Intl.DateTimeFormat` is available in NodeJS but may be fully or partially supported depending on the completeness of the ICU data installed on the system. It would required to review it on the docker image and some additional logic would be required to avoid rendering different date formats in the server default locale and in the client’s one. Rendering at server time based on the `Accept-Language` HTTP header instead of the server default locale to make sure both server and client renders the same would be required to serve a proper user experience.

* How to adopt the same behavior in other platforms lacking `Int.DateTimeFormat` support, for example in back-end services or emails. For backend, an example could be using Carbon library along with the user locale, but it might not be the same format as `Intl.DateTimeFormat`