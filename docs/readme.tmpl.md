# A-Frame Resonance Audio Component

**[👉 Live demo 👈][link-gh-pages]**

**With Resonance Audio, bring dynamic spatial sound into your A-Frame VR, AR experiences at scale.**

[![License][img-license-badge]][link-license]
[![A-Frame Version][img-aframe-badge]][link-aframe]

**Install**  
[![npm version][img-npm-version-badge]][link-npm-package]
[![npm package][img-npm-download-badge]][link-npm-package]

![npm bundle size][img-bundle-size-min-badge]
![npm bundle size][img-bundle-size-minzip-badge]

```
npm i -S aframe-resonance-audio-component
# Or
yarn add aframe-resonance-audio-component
```

JS**DELIVR**  
[![A free CDN for Open Source][img-jsdelivr-badge]][link-jsdelivr]

**Use in version v<%= version %>**
```html
<script src="https://cdn.jsdelivr.net/npm/aframe-resonance-audio-component@<%= version %>/dist/aframe-resonance-audio-component.min.js"></script>
```
**Use in browser latest version** *(not recommended for production usage)*
```html
<script src="https://cdn.jsdelivr.net/npm/aframe-resonance-audio-component@latest/dist/aframe-resonance-audio-component.min.js"></script>
```

| **Builds** | | |
| ----- | ---- | ------- |
| [![linux][img-build-linux-badge]][link-build-linux] | [![macos][img-build-macos-badge]][link-build-macos] | [![windows][img-build-windows-badge]][link-build-windows] |

---

![dependencies](https://img.shields.io/david/mkungla/aframe-resonance-audio-component?style=flat-square)
![devDependencies](https://img.shields.io/david/dev/mkungla/aframe-resonance-audio-component?style=flat-square)
![GitHub last commit](https://img.shields.io/github/last-commit/mkungla/aframe-resonance-audio-component?style=flat-square)

## Table of contents

- [Usage](#usage)
  - [Declarative usage](#declarative-usage)
    - [Example with basic usage with primitives](#example-with-basic-usage-with-primitives)
    - [Example with entities, models and a bounding box room](#example-with-entities-models-and-a-bounding-box-room)
- [Notes](#notes)
  - [Future work](#future-work)
- [Credits](#credits)

[![Codacy Badge][img-codacy-badge]][link-codacy]

## Usage

[Google Resonance][link-resonance-audio] works with audio rooms and audio sources. The acoustics of each source are affected by the room it is in. For that purpose, this package provides three components:
- resonance-audio-room
- resonance-audio-room-bb
- resonance-audio-src

These can be used as primitives (using the `a-` prefix) or as attributes, except `resonance-audio-room-bb`: it calculates the bounding box of an entity and considers that as the room. It can thus not be used as a primitive.

The audio source can also be a [MediaStream][link-web-api-mediastream].

Audio rooms and audio sources don't necessarily have to have a parent-child relationship. See resonance-audio-src's `room` property.

### Declarative usage
Basic usage is quite simple and works just like any other A-Frame component. Don't forget to point the `src` attribute to the correct element or resource.

#### Example with basic usage with primitives

```html
<a-scene>
  <a-assets>
    <audio id="track" src="assets/audio/track.mp3"></audio>
  </a-assets>
  <a-resonance-audio-room
    position="0 0 -5"
    width="40"
    height="4"
    depth="4"
    ambisonic-order="3"
    speed-of-sound="343"
    left="brick-bare"
    right="curtain-heavy"
    front="plywood-panel"
    back="glass-thin"
    down="parquet-on-concrete"
    up="acoustic-ceiling-tiles"
    visualize="true">
    <a-resonance-audio-src
      position="-10 0 0"
      src="#track"
      loop="true"
      autoplay="true"
      visualize="true"></a-resonance-audio-src>
    <a-resonance-audio-src
      position="10 0 0"
      src="#track"
      loop="true"
      autoplay="true"
      visualize="true"></a-resonance-audio-src>
  </a-resonance-audio-room>
</a-scene>
```
#### Example with entities, models and a bounding box room

```html
<a-scene>
  <a-assets>
    <a-asset-item id="room-obj" src="assets/models/room/room-model.obj"></a-asset-item>
    <a-asset-item id="room-mtl" src="assets/models/room/room-materials.mtl"></a-asset-item>
    <audio id="track" src="assets/audio/track.mp3"></audio>
  </a-assets>
  <a-entity
    obj-model="obj: #room-obj; mtl: #room-mtl"
    position="0 1 -0.5"
    resonance-audio-room-bb="
      visualize: true;
      left: brick-bare;
      right: transparent;
      front: transparent;
      back: brick-bare;
      down: parquet-on-concrete;
      up: transparent">
    <a-entity
      position="-1.5 -0.5 1.8"
      resonance-audio-src="
        visualize: true;
        src: #track;
        loop: true;
        autoplay: true;"></a-entity>
  </a-entity>
</a-scene>
```

## Notes
Support for custom positioning and rotation for the audio room is omitted due to the necessity to propagate positioning and rotation calculations to its audio source children and the complexities involved with that.

The visuals are now a simple box wireframe for the audio room and a simple sphere wireframe for the audio source. The box is how Google Resonance actually considers the room and all involved calculations, so other or more complex shapes are not possible (yet). Future work for the audio source visualization might take into account its parameters, such as the directivity pattern and source width.

The audio seems to continue to propagate to infinity outside of an audio room when the default directivity pattern is set.

The bounding box feature isn't perfect yet. It currently takes the dimensions of the entity's bounding box and assumes the center point of the entity is the same as the center point of the audio room. This is correct for simple shapes, but might not be correct for more complex models.

Audio rooms (entities with component `resonance-audio-room`) and audio sources (entities with component resonance-audio-src) have a one to many relationship, and only that relationship. Rooms do not have any influence on eachother. The same goes for audio sources and rooms that they are not descendants of, even if they are physically positioned within another audio room. Do not nest rooms. Furthermore, source entities don't have to be immediate room childs: in that case use the `room` property to point to the audio room.

Dynamically changing positioning and rotation of audio source or room container elements is not fully supported.

### Future work
- [] Hook the `HTMLMediaElement.srcObject` interface so no changes to original code are necessary (except for adding the components).
- [] Take scaling into account.
- [] Find a better method of calculating the bounding box in `resonance-audio-room-bb`.

## Credits

[![GitHub contributors][img-contributors-badge]][link-contributors]

<sub>**Original author Etienne Pinchon.**</sub>  
<sup>aframe-resonance-audio-component was forked from [etiennepinchon/aframe-resonance]</sup>

<sub>**Google Resonance Audio project.**</sub>  
<sup>aframe-resonance-audio-component is based on [Google Resonance Audio project][link-resonance-audio]</sub>

<!-- images -->
<% for (var image in images) { %>
[img-<%= image %>]: <%= images[image] %><% } %>

<!-- links -->
<% for (var link in links) { %>
[link-<%= link %>]: <%= links[link] %><% } %>