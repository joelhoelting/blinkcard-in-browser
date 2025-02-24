# UI component

BlinkCard In-browser UI component acts as a UI layer built on top of core SDK. UI component is a customizable HTML element which provides a UI for scanning of various credit or payment cards from images and from camera feed.

One of the main goals of UI component is to simplify integration of BlinkCard in web apps for various use cases, and to provide high quality UX for end-users.

## Table of contents

* [Installation](#installation)
    * [Installation via CDN](#installation-cdn)
    * [Installation via NPM](#installation-npm)
    * [WASM resources](#installation-wasm-resources)
* [Usage](#usage)
    * [Minimal example](#usage-minimal)
    * [Advanced example](#usage-advanced)
    * [Examples and API documentation](#usage-examples-and-api-documentation)
* [Customization](#customization)
    * [UI customization](#customization-ui)
    * [Custom icons](#customization-icons)
    * [Localization](#customization-localization)
        * [RTL support](#customization-rtl-support)

## <a name="installation"></a> Installation

To use the UI component, JS file with custom element must be loaded and WASM engine must be available.

### <a name="installation-cdn"></a> Installation via CDN

```html
<!-- Load custom element via `<script>` tag with fallback for older browsers -->
<script type="module" src="https://unpkg.com/@microblink/blinkcard-in-browser-sdk/ui/dist/blinkcard-in-browser/blinkcard-in-browser.esm.js"></script>
<script nomodule src="https://unpkg.com/@microblink/blinkcard-in-browser-sdk/ui/dist/blinkcard-in-browser.js"></script>

<!-- Custom element is now available and location of WASM engine must be provided -->
<!-- IMPORTANT: location of WASM engine must be an absolute path -->
<blinkcard-in-browser license-key="..." engine-location="https://unpkg.com/@microblink/blinkcard-in-browser-sdk/resources/"></blinkcard-in-browser>
```

*Keep in mind that Unpkg CDN is used for demonstration, it's not intended to be used in production!*

### <a name="installation-npm"></a> Installation via NPM

```sh
# Install latest version of UI component via NPM or Yarn
npm install @microblink/blinkcard-in-browser-sdk # OR yarn add @microblink/blinkcard-in-browser-sdk

# Copy JS file to folder where other JS assets are located
cp -r node_modules/@microblink/blinkcard-in-browser-sdk/ui/dist/* src/public/js/

# Copy WASM resources from SDK to folder where other static assets are located
cp -r node_modules/@microblink/blinkcard-in-browser-sdk/resources/* src/public/assets/
```

```html
<!-- Load custom element via `<script>` tag with fallback for older browsers -->
<script type="module" src="public/js/blinkcard-in-browser/blinkcard-in-browser.esm.js"></script>
<script nomodule src="public/js/blinkcard-in-browser.js"></script>

<!-- Custom element is now available and location of WASM engine must be provided -->
<!-- IMPORTANT: location of WASM engine must be an absolute path
<blinkcard-in-browser license-key="..." engine-location="http://localhost/public/assets/"></blinkcard-in-browser>
```

### <a name="installation-wasm-resources"></a> WASM resources

After adding the BlinkCard UI component to your project, make sure to include all files from BlinkCard In-browser SDK package and its `resources` folder in your distribution. Those files contain compiled WebAssembly module and support JS code.

Do not add those files to the main app bundle, but rather place them on a publicly available location so SDK can load them at the appropriate time. For example, place the resources in `my-angular-app/src/assets/` folder if using `ng new`, or place the resources in `my-react-app/public/` folder if using `create-react-app`.

## <a name="usage"></a> Usage

BlinkCard UI component acts as any other HTML element. It has custom attributes, properties and events.

Required parameters are license key, engine location and one or more recognizers.

* **License key**
    * To use this SDK, a valid license key is required. Please check the main README file of this repository for more information.
* **Engine location**
    * This SDK uses WASM engine in the background for image processing. Please check the [WASM resources](#installation-wasm-resources) section for more information.
* **Recognizer**
    * The `Recognizer` is the basic unit of processing within the BlinkCard SDK. Its main purpose is to process the image and extract meaningful information from it.
    * Please check the main README file of this repository for a list of available recognizers.

### <a name="usage-minimal"></a> Minimal example

```html
<blinkcard-in-browser
    engine-location="http://localhost/resources"
    license-key="<PLACE-YOUR-LICENSE-KEY-HERE>"
    recognizers="BlinkCardRecognizer"
></blinkcard-in-browser>

<script>
    // Register listener for successful scan event to obtain results
    const el = document.querySelector('blinkcard-in-browser');

    el.addEventListener('scanSuccess', ev => {
        // Since UI component uses custom events, data is placed in `detail` property
        console.log('Results', ev.detail);
    });
</script>
```

### <a name="usage-advanced"></a> Advanced example

```html
<!-- UI component can be customized with JS attributes or HTML properties -->
<blinkcard-in-browser></blinkcard-in-browser>

<script>
    const el = document.querySelector('blinkcard-in-browser');

    /**
     * Mandatory properties
     */

    // Absolute path to location of WASM resource files
    el.engineLocation = 'http://localhost/resources';

    // License key
    el.licenseKey = '<PLACE-YOUR-LICENSE-KEY-HERE>';

    // Recognizers - logic which should be used to extract data
    el.recognizers = ['BlinkCardRecognizer'];

    /**
     * Optional properties
     *
     * See docs/components/blinkcard-in-browser/readme.md for more information.
     */
    el.allowHelloMessage     = true;
    el.recognizerOptions     = undefined;
    el.enableDrag            = true;
    el.hideFeedback          = false;
    el.hideLoadingAndErrorUi = false;
    el.scanFromCamera        = true;
    el.scanFromImage         = true;
    el.cameraId              = null;
    el.translations          = undefined;
    el.iconCameraDefault     = undefined;
    el.iconCameraActive      = undefined;
    el.iconGalleryDefault    = undefined;
    el.iconGalleryActive     = undefined;
    el.iconInvalidFormat     = undefined;
    el.iconSpinner           = undefined;

    el.translations = {
        'action-message': 'Alternative CTA'
    }

    /**
     * Events
     */

    // Event emitted when UI component cannot initialize
    el.addEventListener('fatalError', ev => {
        console.log('fatalError', ev.detail);
    });

    // Event emitted when UI component is ready to use
    el.addEventListener('ready', ev => {
        console.log('ready', ev.detail);
    });

    // Event emitted in case of error during scan action
    el.addEventListener('scanError', ev => {
        console.log('scanError', ev.detail);
    });

    // Event emitted when scan is successful
    el.addEventListener('scanSuccess', ev => {
        console.log('scanSuccess', ev.detail);
    });

    // Event emitted when UI component wants to display a feedback message to the user
    el.addEventListener('feedback', ev => {
        console.log('feedback', ev.detail);
    });
</script>
```

### <a name="usage-examples-and-api-documentation"></a> Examples and API documentation

A demo app with multiple UI components alongside with source code can be found in the [demo.html](demo.html) file.

Example apps are located in the [examples](examples) directory, where minimal JavaScript example is located in the [examples/javascript](examples/javascript) directory, while the minimal TypeScript example is located in the [examples/typescript](examples/typescript) directory.

Auto-generated API documentation of UI component is located in the [docs](docs) directory.

## <a name="customization"></a> Customization

All attributes, properties and events of UI component can be seen in [`<blinkcard-in-browser>` API documentation](docs/components/blinkcard-in-browser/readme.md).

### <a name="customization-ui"></a> UI customization

UI component relies on CSS variables which can be used to override the default styles.

All CSS variables are defined in [\_globals.scss](src/components/shared/styles/_globals.scss) file.

```css
/**
 * Example code which modifies default values of CSS variables used by an
 * instance of UI component.
 */
blinkcard-in-browser {
    --mb-font-family:           inherit;
    --mb-component-background:  #FFF;
    --mb-component-font-color:  #000;
    --mb-component-font-size:   14px;
}
```

### <a name="customization-icons"></a> Custom icons

It's possible to change the default icons used by the UI component during configuration.

```javascript
const el = document.querySelector('blinkcard-in-browser');

// Value provided to this property will be used for setting the `src` attribute
// of <img> element.
el.iconSpinner = '/images/icon-spinner.gif';
```

For a full list of customizable icons, see [`<blinkcard-in-browser>` API documentation](docs/components/blinkcard-in-browser/readme.md).

### <a name="customization-localization"></a> Localization

It's possible to override the default messages defined in the [translation.service.ts](src/utils/translation.service.ts) file.

```javascript
const el = document.querySelector('blinkcard-in-browser');

el.translations = {
    'action-message': 'Alternative CTA',

    // During the camera scan action, messages can be split in multiple lines by
    // providing array of strings instead of a plain string.
    'camera-feedback-scan-front': ['Place the front side', 'of a document']
}
```

#### <a name="customization-rtl-support"></a> RTL support

To use UI component in RTL interfaces, explicitly set `dir="rtl"` attribute on HTML element.

```html
<blinkcard-in-browser ... dir="rtl"></blinkcard-in-browser>
```
