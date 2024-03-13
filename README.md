# A-Frame Custom Patch for 8th Wall Integration

**Authors:** Jeff Burke, Aidan Strong, Ryan Yeo

**Date:** 3/1/2024

## Purpose

For the USITT 24 WebAR Demo, we leverage both 8Frame and the A-Frame Inspector, making changes accordingly to fit the needs of this project.

This patch of the A-Frame inspector supports development on our [USITT 23 WebAR Demo](https://www.8thwall.com/decentar/usitt-template/project), enabling developers to create augmented reality (AR) experiences using A-Frame within the 8th Wall platform.

### 8Frame

8Frame is 8th Wall's modified version of AFrame, with the following changes to improve compatibility with their platform:

- 8Frame exposes properties in THREE.js's `WebGLRenderer` so that it's compatible with MRCS HoloVideoObject.
- 8Frame improves AFrame's compatibility with XR8.

### A-Frame Inspector

A-Frame comes with a **keyboard shortcut** to inject the inspector. Just open
up any A-Frame scene (running at least A-Frame v0.3.0) and press **`<ctrl> +
<alt> + i`** to inject the inspector, just like you would use a DOM inspector:

- [Documentation / Guide](https://aframe.io/docs/master/introduction/visual-inspector-and-dev-tools.html)
- [Example](https://aframe.io/aframe-inspector/examples/)

## What's Different

The patch modifies certain aspects of A-Frame while ensuring compatibility with 8th Wall's AR platform, which is helpful for the AR development workflow on 8th Wall. A summary of the modifications:

- useful property updates via console in response to user interactions
- toolbar addition that provides a convenient way for users to copy entity HTML to the clipboard

For further details, see [changes.md](./changes.md).

## How to use our Inspector

To immediately use our modification of the A-Frame Inspector on your 8th Wall project, add the following to your `<a-scene>` in `body.html`:

` inspector="url: https://ryandc-yeo.github.io/aframe-inspector/dist/inspector.js"`

## Making Your Own Modifications

If you wish to make your own changes to our A-Frame Inspector, follow these steps:

1. Visit the [REMAP A-Frame Inspector GitHub repository](https://github.com/remap/aframe-inspector).
2. **Fork** the respository then download it to your local machine.
3. Navigate to the `/dist` directory containing the A-Frame Inspector source code in `inspector.js`.
4. Test if your local copy of the inspector works by running it (_npm start_), then in your 8th Wall repository, add the following to your `<a-scene>` in `body.html`:
   `inspector="url: http://localhost:3333/dist/inspector.js"`
5. Preview your project in a new window and open the A-Frame inspector with `<ctrl> + <alt> + i`. On the left panel, choose `<a-scene_scene>` and a panel on the right should show up. Click on INSPECTOR and make sure the url of the inspector matches the source (http://localhost:3333/dist/inspector.js).
6. Now you can make your modifications, **make sure the changes are compatible with A-Frame and 8th Wall**.
7. Once you are happy with the changes, publish your repository on Github Pages, then replace the link in your 8th Wall project accordingly.
