# FAQ

## What is Open Source?

Open Source started as the Free Software movement, which believes users should "have the freedom to run, copy, distribute, study, change and improve the software." If you ask 10 people about the difference between Open Source and Free Software, you'll likely get 10 different answers. While the philosophy can be vague, in practice, this translates to software licenses, which are clear and legally binding. 

According to the Open Source Initiative, in order for a software license to count as open source, it has to allow for freedom of redistribution, distribution of source code (both in text and compiled form), and modified/derived works. The licence cannot discriminate against any groups of people, or against any use cases. It also cannot restrict any other software that might be bundled with the open source program as part of a larget product.

The Webstudio Designer is distributed under the MIT license, an Open Source license that allows the user to pretty much do what they want with the software (copy, modify, merge, publish, distrubute, sublicense, or sell) as long as the same copyright/permission notice is included—essentially, do what you want, but give credit. This is the most popular Open Source license as it is a simple license that allows orgs to safely share their source code with minimum legal headache.

Many products use a combination of Open Source and proprietary parts, and as such can't be called Open Source. Some companies choose to differentiate the open source components vs the entire product (e.g. The Android on your Pixel phone is proprietary, but within it is the Android Open Source Project (AOSP) which is Open Source). Some companies choose instead to call their products Open Core, which says that the "core" of the product is Open Source, but acknowledges the existence of proprietary parts within. In our case, while the Webstudio Designer is open source, Webstudio as a whole would be Open Core.

[Open Source Definition](https://opensource.org/osd), [MIT License](https://mit-license.org/)

## How does Edge Deployment work?

Cloudflare allows you to "deploy serverless code instantly across the globe to give it exceptional performance, reliability, and scale." What does that mean?

In the old days, you had to manage your own servers (actual machines) meaning deploying your code on your own equipment. This is expensive, labor intensive, and takes resources away from writing business logic. These days we do it with Edge Deployment, which deploys code using Content Delivery Networks (CDNs), which are systems of servers (virtual machines that run on actual machines) that deliver web content to users based on their geographic location, allowing for faster and more efficient delivery of web pages.

When a user does something, it registers as an event (e.g clicks on a sign-up button), which the CDN responds to by executing the relavent function (e.g. returns the sign-up page). This is called "function as a service (FNAS)" Instead of buying a fixed amount of server space, businesses pay CDNs for functions executed.

Examples of CDNs include AWS Lamda, and Cloudflare Workers. The difference between AWS Lamda and Cloudflare workers is that AWS Lamda runs full Node.JS, and Cloudflare Workers use Workers (a Javascript virtual machine + APIs) that run inside a V8 browser (the same tech used in Google Chrome). The advantage of running full Node.JS is there is full compatibility, but it's slower as the Node.JS code takes time to start up (known as the cold start problem), keeping the Node.JS code running and listening for more events solves this, but is resource intensive. The benefit of Workers in the browser is that they start up much faster, and thus can be turned off once the function is executed. Cloudflare has also open-sourced its Workers technology (workerd). So someone could create their own Workers system without Cloudlfare.

[Cloudflare Workers](https://workers.cloudflare.com/), [Cloudflare's open source workerd technology](https://blog.cloudflare.com/workerd-open-source-workers-runtime/)

## What are Design Tokens?

The larger a design system gets, the harder it is to keep things consistent. Colors, spacing, border-widths, font-sizes, etc, can become a nightmare to upkeep, especially without documentation (e.g. use the eyedropper tool to find the color of the button). Even with good design documentation, it can be unclear which value goes where (e.g. is the button supposed to be blue-400 or blue-500?), and every time something new is implemented is a chance to muddy the system.

Design Tokens solve this problem by creating a single source of truth for designers and developers. Variables that dictate font-size, border-widths, color, spacing, etc are stored as tokens, and are accessible to team members via their tools of choice (through JSON files and sync providers such as Github). A designer can change a design token on Figma, and a developer will see that change on Visual Studio Code. 

A common use case for Design Tokens is semantic colors, where colors are named after their use (e.g. the button color is $button.primary, and the hex-code for $button.primary is defined in the token).

Because Design Tokens are essentially stored variables, they can do lots of things. Tokens can reference other tokens (e.g. $button.primary is defined as the same value as $blue-500 and the hex-code for $blue-500 is #0000FF). Tokens can be changed in specific situations (e.g in dark mode, let $button.primary be $blue.400). Tokens can also be manipulated with math (e.g. $border-width.2=$border-width.1*2).

Design Tokens don't replace design documentation, but are used in conjunction to create a design system that is easily implemented and maintained.

[WC3 Design Tokens Format](https://tr.designtokens.org/format/), [Tokens Studio Plugin documentation](https://docs.tokens.studio/)

## How does the GDPR protect data privacy? (What counts as GDPR compliant?)

The General Data Protection Regulation (GDPR) is the toughest data privacy and security law in the world. This law applies to organizations everywhere that target and collect data from people in the EU. Fines for violating the GDPR can go up to €20 million or 4% of the company's global revenue (whichever is higher).

The goal of the GDPR is to ensure that EU citizens' privacy rights are protected. Those privacy rights are as follows:

1. The right to be informed
2. The right of access
3. The right to rectification
4. The right to erasure
5. The right to restrict processing
6. The right to data portability
7. The right to object
8. Rights in relation to automated decision making and profiling.

In order to do so, companies are expected to process data according to 7 protection and accountability principles:

1. Lawfulness, fairness and transparency — Processing must be lawful, fair, and transparent to the data subject.
2. Purpose limitation — You must process data for the legitimate purposes specified explicitly to the data subject when you collected it.
3. Data minimization — You should collect and process only as much data as absolutely necessary for the purposes specified.
4. Accuracy — You must keep personal data accurate and up to date.
5. Storage limitation — You may only store personally identifying data for as long as necessary for the specified purpose.
6. Integrity and confidentiality — Processing must be done in such a way as to ensure appropriate security, integrity, and confidentiality (e.g. by using encryption).
7. Accountability — The data controller (the org) is responsible for being able to demonstrate GDPR compliance with all of these principles. (orgs with less than 250 employees are exempt from some of the record keeping this entails)

Overall, it is very easy to violate GDPR compliance. Common practices like using Google Fonts (note it's not that Google Fonts wants to spy on you, they just need your IP address to know where to send the font to), and storing users data in US based servers could be enough to be found non-compliant. We'll probably want to consult an expert to ensure that we comply. 

Considering that our product is designed to be extensible, we can't guarantee that every website made with Webstudio will be GDPR compliant, but we will make it so that there is nothing inherent about our product that violates GDPR.

[What is GDPR?](https://gdpr.eu/what-is-gdpr/), [Who must comply with GDPR](https://gdpr.eu/companies-outside-of-europe/)

## How does Webstudio optimize images?

Images comprise up to 60%-65% of bytes on most web pages and page size is a major factor in total rendering time. In order to improve page load, Google created the WebP image format. On average WebP images are about 30% smaller than equivalent PNG/JPEG images. In addition, WebP is also an Open Source format.

WebP supports both lossless and lossy image formats (lossless retains the exact color data of each pixel but you end up with a bigger file, lossy trades some of that color accuracy for smaller file sizes), as well as alpha channels (transparency), ICC Color profiles (which tell a computer/printer how to manage colors), metadata (EXIF and XMP data that provide context information such as where and when a photo was shot), and animation (frame animations similar to GIFs). All this together makes WebP both a lean and versatile format.

Formats aside, another determining factor of image size is its dimensions. All else being equal, a 100px X 100px image is always going to be smaller than a 500px X 500px image. Often, designers choose image dimensions according to the biggest use case. While this might be great for large desktops, this leads to wasted megabytes when loading the same image on the phone. This can be optimized by uploading images with different dimensions for different screens but this is additional work. 

In addition to automatically converting images to WebP, Webstudio detects the exact dimensions the image needs to fit into on the webpage, and resizes the image accordingly. Both of these steps ensure that images are as small as possible without degrading the viewing experience.

[WebP FAQ](https://developers.google.com/speed/webp/faq)

## How is Webstudio Extensible? (What is an API?)

How does your weather app know what the whether is like? How does Booking.com know the rates of all those hotels and airlines? How do websites allow you to login via Google/Twitter/Facebook? With APIs!

Application Programing Interfaces (APIs) are how apps exchange information. Usually this communication takes place between a client app and a server app. E.g. the weather app on your phone (the client) uses an API to retrieve weather data from an online source—like Accuweather (the server). There are 3 major types of API: Private APIs for internal use within orgs; Public APIs used by companies to make their products/services available to the public; and Partner APIs that are accessible to people within an org and authorized partners.

Webstudio provides public APIs to allow developers to retrieve information about:
1. Component Tree
2. CSS
3. Images
4. Fonts
5. Interactions
6. Events

Webstudio uses APIs to allow developers to extend its functionality to fit their needs. It provides an Integration API that allows developers to create their own UIs within Webstudio, similar with Figma plugins. In addition, developers can use APIs to provide metadata about their components.

[API Wiki](https://en.wikipedia.org/wiki/API)

## What is Webstudio trying to be?

Beyond the beta, Webstudio is focusing on becoming the best tool for creating user interfaces for the web. Today it is capable of building static websites that are fast and responsive. Additional features like the linked CSS editor, animations and real-time collaboration are coming. 

Using APIs, users can integrate Webstudip with services of their choice, such as CMS, E-commerce platforms, Automation tools, Databases, etc. As much as possible, Webflow uses Open Source components and standards to give users the freedom to create whatever they want, however they want.

[Webstudio Vision Document](https://webstudiois.notion.site/Vision-f52ed097ccaa410eb05076981d446c2f)

Fin.
