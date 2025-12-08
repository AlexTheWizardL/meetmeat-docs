# Canvas/Editor Libraries Research for Visual Poster Editor
## React Native + Web Support (iOS, Android, Web)

**Research Date:** December 8, 2025
**Target App:** MeetMeAt Poster Editor
**Requirements:**
- Cross-platform: iOS, Android, Web (via React Native Web)
- Interactive elements: select, drag, resize, rotate, color changes
- Inline text editing
- Export to PNG/JPG
- Active maintenance and good documentation

---

## Executive Summary

After comprehensive research of 30+ libraries and approaches, the **recommended solution** is:

### Primary Recommendation: React Native Skia + Gesture Handler
**Best for:** Production-ready, high-performance cross-platform editor

**Stack:**
- `@shopify/react-native-skia` (canvas rendering)
- `react-native-gesture-handler` (touch interactions)
- `react-native-reanimated` (animations)

**Why:** Native performance on mobile (60-120 FPS), excellent web support via WebAssembly/CanvasKit, full control over rendering, active maintenance by Shopify, and proven in production apps.

### Alternative (For faster MVP): Polotno SDK + WebView Hybrid
**Best for:** Quick MVP with pre-built editor UI

**Why:** Complete editor out-of-the-box, similar to Canva, but requires commercial license ($155/month) and less native feel.

---

## 1. React Native Specific Solutions

### 1.1 React Native Skia ⭐ RECOMMENDED

**Repository:** https://github.com/Shopify/react-native-skia
**Stars:** ~8k (GitHub trending)
**Maintenance:** Actively maintained by Shopify, latest update Nov 2025
**License:** MIT

#### Overview
React Native Skia brings Google's Skia 2D graphics library to React Native. Skia is the same graphics engine used in Chrome, Android, and Flutter, providing native-level performance for 2D graphics.

#### Key Features
- High-performance 60-120 FPS canvas rendering on native and web
- Declarative API with React components (`<Canvas>`, `<Group>`, `<Rect>`, `<Text>`, etc.)
- Imperative API for complex drawing operations
- Built-in gesture support via integration with react-native-gesture-handler
- Web support via CanvasKit (Skia compiled to WebAssembly)
- Image export via `makeImageSnapshot()` and `makeImageSnapshotAsync()`
- Supports PNG, JPEG, and WebP export formats
- Works with Expo (managed workflow)

#### Pros for Our Use Case
- **Native performance**: Runs on UI thread, not JS thread
- **Cross-platform**: Same code works on iOS, Android, and Web
- **Full control**: Build custom editor exactly to requirements
- **Production-ready**: Used by major apps (Shopify, Notesnook)
- **Active community**: Frequent updates, responsive maintainers
- **TypeScript support**: Full type definitions included
- **No licensing fees**: MIT license, completely free

#### Cons
- **Higher complexity**: Need to build editor UI from scratch
- **Learning curve**: New API compared to HTML canvas
- **Manual state management**: Need to handle selection, undo/redo yourself

#### Implementation Approach

**Core Stack:**
```json
{
  "@shopify/react-native-skia": "^2.0.0",
  "react-native-gesture-handler": "^2.0.0",
  "react-native-reanimated": "^3.0.0"
}
```

**Architecture:**
1. **Canvas Layer**: Skia canvas for rendering all visual elements
2. **State Management**: Store poster elements (images, text, shapes) in state
3. **Gesture Layer**: Overlay gesture handlers for selection/dragging
4. **Transform Layer**: Handle pinch-to-zoom, rotation with reanimated
5. **Export Layer**: Use `makeImageSnapshotAsync()` for PNG/JPG export

**Example Code Structure:**
```javascript
import { Canvas, Image, Text, Group } from '@shopify/react-native-skia';
import { GestureDetector, Gesture } from 'react-native-gesture-handler';

const PosterEditor = () => {
  const [elements, setElements] = useState([]);
  const selectedElement = useSharedValue(null);

  const panGesture = Gesture.Pan()
    .onUpdate((e) => {
      // Update element position
    });

  const pinchGesture = Gesture.Pinch()
    .onUpdate((e) => {
      // Handle resize/zoom
    });

  return (
    <GestureDetector gesture={Gesture.Simultaneous(panGesture, pinchGesture)}>
      <Canvas style={{ flex: 1 }}>
        {elements.map(element => (
          <Group key={element.id} transform={element.transform}>
            {element.type === 'text' ? (
              <Text {...element.props} />
            ) : (
              <Image {...element.props} />
            )}
          </Group>
        ))}
      </Canvas>
    </GestureDetector>
  );
};
```

**Export Implementation:**
```javascript
import { useCanvasRef, ImageFormat } from '@shopify/react-native-skia';

const canvasRef = useCanvasRef();

const exportToPNG = async () => {
  const image = await canvasRef.current?.makeImageSnapshotAsync();
  if (image) {
    const base64 = image.encodeToBase64(ImageFormat.PNG, 100);
    // Save or share the base64 image
  }
};
```

#### Similar Implementations
- **Notesnook Drawing App**: 60 FPS freehand drawing with Skia
- **react-native-free-canvas**: High-performance sketching with zoom/pan
- **@equinor/react-native-skia-draw**: Signature pad and image markup

#### Resources
- [Official Documentation](https://shopify.github.io/react-native-skia/)
- [Getting Started with React Native Skia](https://shopify.engineering/getting-started-with-react-native-skia)
- [Building a 60 FPS Drawing App](https://blog.notesnook.com/drawing-app-with-react-native-skia/)
- [React Native Skia for Web Graphics (Expo 2026)](https://dev.to/sherry_walker_bba406fb339/using-react-native-skia-for-web-graphics-with-expo-2026-3chj)

---

### 1.2 react-native-free-canvas

**Repository:** https://github.com/doublelam/react-native-free-canvas
**Stars:** ~100-200 (newer project)
**Maintenance:** Active, built on react-native-skia
**License:** MIT

#### Overview
High-performance drawing and sketching library built on top of react-native-skia with built-in zoom and pan functionality.

#### Key Features
- Built on react-native-skia foundation
- Pre-implemented zoom and pan gestures
- Smooth drawing performance
- Path effects support (corner smoothing, etc.)
- Optimized for freehand sketching

#### Pros
- Simpler API than raw Skia for drawing use cases
- Built-in zoom/pan saves implementation time
- Still gets Skia's performance benefits

#### Cons
- More focused on drawing/sketching than general poster editing
- Less flexible than using Skia directly
- Smaller community and fewer examples
- May need to extend for text, images, shapes

---

### 1.3 react-native-canvas

**Repository:** https://github.com/iddan/react-native-canvas
**NPM:** https://www.npmjs.com/package/react-native-canvas
**Stars:** ~600
**Maintenance:** Limited (last major update 2018-2020)
**License:** MIT

#### Overview
HTML5 Canvas API polyfill for React Native using react-native-webview under the hood.

#### Key Features
- Familiar HTML5 Canvas API
- Works via WebView rendering
- Supports 2D context operations
- Async/await for method calls

#### Pros
- Familiar API if you know HTML5 Canvas
- Can reuse web canvas code

#### Cons
- **Performance concerns**: Runs in WebView, not native
- **Async overhead**: All canvas operations require await
- **Limited maintenance**: Not actively developed
- **WebView dependency**: Adds complexity and performance overhead
- **Not truly native**: Defeats purpose of React Native

**Verdict:** Not recommended. Better to use WebView hybrid approach with full-featured library if going the WebView route.

---

### 1.4 react-native-sketch-canvas

**Repository:** https://github.com/terrylinla/react-native-sketch-canvas
**Fork:** https://github.com/sourcetoad/react-native-sketch-canvas (maintained)
**Stars:** ~600 (original), ~50 (fork)
**Maintenance:** Original abandoned 2018, fork active
**License:** MIT

#### Overview
Drawing component for iOS and Android with touch-based sketching.

#### Key Features
- Touch-based drawing
- Eraser mode (transparent stroke color)
- Path storage
- Native implementation

#### Pros
- Simple API for basic drawing
- Native performance

#### Cons
- Focused on sketching, not general editing
- No built-in text, image, or shape support
- Limited to drawing use cases
- Smaller feature set than Skia

---

### 1.5 react-native-svg

**Repository:** https://github.com/software-mansion/react-native-svg
**Stars:** ~7.2k
**Maintenance:** Very active (Software Mansion)
**License:** MIT

#### Overview
SVG rendering library for React Native with declarative API.

#### Key Features
- Declarative SVG components
- Cross-platform (iOS, Android, Web)
- Good performance for vector graphics
- Excellent for static or animated SVGs

#### Pros
- Well-maintained and widely used
- Great for vector graphics
- Works with react-native-web

#### Cons
- **Not ideal for interactive editing**: No built-in drag/drop
- **Gesture integration complex**: Need manual PanResponder or gesture-handler setup
- **Performance limits**: Not optimized for many interactive elements
- **Missing editor features**: No selection, resize handles, etc.

**Verdict:** Good for rendering static poster output, but not ideal as the primary editor canvas. Could be used in combination with other solutions.

---

## 2. Web Canvas Libraries with RN Web Compatibility

### 2.1 Fabric.js ⚠️ LIMITED RN SUPPORT

**Repository:** https://github.com/fabricjs/fabric.js
**Website:** https://fabricjs.com/
**Stars:** ~30k
**Maintenance:** Very active, v6.9.0 released 2025
**License:** MIT
**NPM Downloads:** 1.43M/month

#### Overview
Powerful HTML5 canvas library with rich object model, SVG-to-canvas parser, and interactive features.

#### Key Features
- Object-oriented canvas manipulation
- Built-in interactive controls (select, drag, resize, rotate)
- Text editing with rich styling
- Image filters and effects
- SVG import/export
- Object serialization/deserialization
- Event system for interactions
- TypeScript support
- No dependencies, 95.7 kB minified+gzipped

#### Pros (for Web)
- **Mature and battle-tested**: 30k+ stars, 10+ years
- **Feature-complete**: Has everything needed for poster editor
- **Active development**: Recent security fixes and improvements
- **Rich documentation**: Extensive examples and tutorials
- **Mobile web support**: Touch events, pinch zoom via extensions

#### Cons (for React Native)
- **No native React Native support**: Requires HTML5 Canvas API
- **Works only with react-native-web**: On web platform only
- **Requires WebView on mobile**: If you want it on iOS/Android native
- **Performance on mobile WebView**: Not as good as native solutions

#### Mobile Touch Support
Fabric.js doesn't have built-in mobile gestures but can be extended:
- **fabric-with-gestures**: NPM package adding pinch/rotate support
- **Custom implementations**: Using HammerJS or AlloyFinger
- **Manual event handling**: Detecting ctrlKey for pinch events

#### React Native Web Compatibility
- ✅ Works on web via react-native-web
- ❌ Does NOT work on iOS/Android native
- ⚠️ Could work via WebView hybrid approach (not recommended)

#### Implementation Approach for RN Web
```javascript
// Only works on web platform
import { fabric } from 'fabric';
import { Platform } from 'react-native';

if (Platform.OS === 'web') {
  const canvas = new fabric.Canvas('canvas');
  canvas.add(new fabric.Text('Hello'));
  // Full Fabric.js functionality available
} else {
  // Need alternative for iOS/Android
}
```

**Verdict:** Excellent for web-only projects, but NOT suitable for true React Native cross-platform. Would require maintaining two separate implementations (Fabric.js for web, something else for mobile).

---

### 2.2 Konva.js / react-konva ⚠️ WEB ONLY

**Repository:** https://github.com/konvajs/konva
**React Wrapper:** https://github.com/konvajs/react-konva
**Website:** https://konvajs.org/
**Stars:** ~13.8k (konva), 6.2k (react-konva)
**Maintenance:** Very active, updated Nov 2025
**License:** MIT

#### Overview
HTML5 Canvas JavaScript framework extending 2D context with interactivity for desktop and mobile applications.

#### Key Features
- Declarative canvas API for React
- Layering system (Stage → Layer → Shape)
- Built-in drag and drop
- Event handling (click, touchmove, dragend)
- Filters and effects
- Animation support
- High performance with caching
- Touch support for mobile web browsers

#### Pros (for Web)
- **React-friendly**: react-konva provides JSX components
- **Good performance**: Object caching, layering optimizations
- **Active maintenance**: Updated Nov 2025
- **Touch events**: onTap, onTouchMove for mobile web
- **Comprehensive tests**: Hundreds of tests with 1000+ assertions

#### Cons (for React Native)
- **NOT supported in React Native environment**: Official docs confirm
- **HTML5 Canvas dependency**: No native RN renderer available
- **Web only**: Works in mobile browsers but not native apps

#### React Konva API
```javascript
import { Stage, Layer, Rect, Text, Circle } from 'react-konva';

<Stage width={window.innerWidth} height={window.innerHeight}>
  <Layer>
    <Text text="Hello" />
    <Circle x={100} y={100} radius={50} fill="red" draggable />
  </Layer>
</Stage>
```

**Verdict:** Excellent for web-based poster editor, but NOT compatible with React Native iOS/Android. Same limitation as Fabric.js.

---

### 2.3 Paper.js ⚠️ NO RN SUPPORT

**Repository:** https://github.com/paperjs/paper.js
**Website:** http://paperjs.org/
**Stars:** ~14k+
**Maintenance:** Active but slower pace
**License:** MIT

#### Overview
Vector graphics scripting framework for HTML5 Canvas.

#### Key Features
- Clean vector graphics API
- Path manipulation
- Geometric operations
- Scene graph architecture

#### Cons
- ❌ No React Native support (GitHub issue from 2017 unresolved)
- HTML5 Canvas dependency
- Less interactive features than Fabric.js/Konva

**Verdict:** Not suitable for React Native. Better alternatives exist even for web-only projects.

---

### 2.4 PixiJS ⚠️ LIMITED RN SUPPORT

**Repository:** https://github.com/pixijs/pixi.js
**React Wrapper:** https://react.pixijs.io/
**Stars:** ~43k+
**Maintenance:** Very active, React v8 released March 2025
**License:** MIT

#### Overview
WebGL-based 2D rendering engine for games and rich interactive graphics.

#### Key Features
- WebGL acceleration (faster than Canvas 2D)
- Rich plugin ecosystem
- Sprite handling
- Advanced rendering features

#### React Native Options
- **expo-pixi**: Wrapper for Expo projects
- **react-native-pixi**: Bridge implementation (v3+)
- **pixi-native**: Experimental native version

#### Pros
- Very high performance (WebGL-based)
- React v8 with excellent TypeScript support

#### Cons
- **Primarily for games**: Overkill for poster editor
- **Complex setup for RN**: Requires additional bridge layers
- **No clear RN Web story**: react-native-web compatibility unclear
- **Heavy dependency**: Larger bundle size than canvas libraries

**Verdict:** Better suited for games and rich interactive graphics. Too complex for poster editor use case. React Native support is experimental and not production-ready.

---

## 3. Design Tool Clones / Open-Source Editors

### 3.1 Polotno Studio / SDK ⭐ COMMERCIAL OPTION

**Repository:** https://github.com/polotno-project/polotno-studio
**Website:** https://polotno.com/
**SDK Docs:** https://polotno.com/docs/overview
**License:** Commercial (free trial for dev, paid for production)

#### Overview
Complete Canva-like design editor built with React and Konva.js. Available as open-source studio (demo) and commercial SDK (full-featured).

#### Key Features
- Full design editor UI out-of-the-box
- Text editing with rich formatting
- Image manipulation and filters
- Templates and design elements
- Asset management and integrations
- Export to PNG, JPG, PDF
- Undo/redo built-in
- Customizable React components

#### Pricing
- **Free trial**: 100 days for development/staging only
- **Team**: $155.83/month (remove Polotno branding)
- **Business**: $332.50/month
- **Custom**: Enterprise pricing

#### Pros
- **Fastest to market**: Complete editor ready to use
- **Professional UI**: Canva-like interface users will recognize
- **Feature-complete**: Everything needed for poster editing
- **Good documentation**: Comprehensive SDK docs
- **React-based**: Easy to integrate and customize

#### Cons
- **Web only**: No native iOS/Android support (Konva.js based)
- **Licensing cost**: $155+/month for production
- **Opinionated design**: UI structure is pre-defined
- **Vendor lock-in**: Building on proprietary SDK
- **Attribution required**: Free version shows "Powered by Polotno"

#### React Native Compatibility
- ❌ Not compatible with React Native (Konva.js dependency)
- ✅ Could work via WebView embedding for hybrid approach
- ⚠️ Better as standalone web editor than embedded in RN app

#### Use Case Fit
**Best for:** Quick MVP where you want a web-based editor and can afford licensing.
**Not ideal for:** True native mobile experience or if you need fine-grained control.

---

### 3.2 Open-Source Canva Clones (React + Fabric.js)

Several open-source Canva-like editors available on GitHub:

#### a) ajpgtech/design-editor
**Repository:** https://github.com/ajpgtech/design-editor
**Stack:** React + Fabric.js
**Features:** Image creation, diagrams, compositions, export formats

#### b) Mural Design Editor
**Stars:** ~129
**Last Update:** Nov 4, 2025
**Stack:** FabricJS + React + TypeScript
**Features:** Similar to Canva.com, multiple export options

#### c) imgly/canva-clone-react-cesdk
**Repository:** https://github.com/imgly/canva-clone-react-cesdk
**Stack:** React + IMG.LY CE.SDK
**Features:** Invitations, greeting cards, flyers, business cards
**License:** AGPL-3.0 (code) + Commercial required (CE.SDK)

#### d) designcombo/react-video-editor
**Repository:** https://github.com/designcombo/react-video-editor
**Stack:** React + Remotion
**Features:** Video editing (CapCut/Canva clone for video)

#### Verdict on Open-Source Clones
- All are **web-only** (Fabric.js or Konva.js based)
- Good starting point for understanding editor architecture
- Would need significant work to adapt for React Native
- Better to study and learn from than directly use

---

### 3.3 IMG.LY Creative SDK (Commercial Alternative)

**Website:** https://img.ly/
**Comparison:** https://img.ly/polotno-alternative

#### Overview
Commercial alternative to Polotno with broader capabilities.

#### Key Features
- Cross-platform SDK (includes mobile)
- Unified SDK across all deployment targets
- Headless mode for automation
- Better asset management
- Direct integrations (Getty, Unsplash, Airtable)

#### Pricing
- More expensive than Polotno
- Zero-branding by default
- Enterprise-focused

**Verdict:** More comprehensive but also more expensive. Consider if budget allows and need enterprise features.

---

## 4. Hybrid Approaches

### 4.1 WebView + Web-Based Editor

#### Concept
Embed a full-featured web canvas editor (Fabric.js, Konva, or Polotno) inside a React Native WebView.

#### Implementation
```javascript
import { WebView } from 'react-native-webview';

const EditorScreen = () => {
  return (
    <WebView
      source={{ uri: 'https://your-editor.com' }}
      // or inline HTML with editor library
    />
  );
};
```

#### Pros
- **Leverage mature web libraries**: Use Fabric.js, Konva.js, etc.
- **Faster development**: Reuse existing web editor code
- **Feature-rich**: Access to all web canvas features
- **Cross-platform**: Same code on iOS/Android/Web

#### Cons
- **Performance**: WebView overhead, not native performance
- **Communication overhead**: Bridge between RN and WebView via postMessage
- **UX concerns**: May not feel fully native
- **Debugging complexity**: Issues span RN and WebView
- **Memory usage**: WebView can be heavy

#### Best For
- MVP where speed to market is critical
- When you already have a working web editor
- Less CPU/GPU intensive screens
- When native performance isn't critical

#### Not Recommended For
- Graphics-intensive apps (games, video)
- When native feel is important
- Performance-critical applications

---

### 4.2 Platform-Specific Implementations

#### Concept
Use different libraries for different platforms:
- **Web**: Fabric.js or Konva via react-native-web
- **iOS/Android**: React Native Skia

#### Implementation
```javascript
import { Platform } from 'react-native';

const Editor = Platform.select({
  web: () => require('./WebEditor').default,  // Fabric.js
  default: () => require('./NativeEditor').default, // Skia
})();
```

#### Pros
- **Best of both worlds**: Use optimal library per platform
- **Native performance on mobile**: Skia for iOS/Android
- **Rich features on web**: Mature Fabric.js for web

#### Cons
- **2x maintenance**: Two separate codebases to maintain
- **Feature parity challenges**: Keeping platforms in sync
- **Testing overhead**: Must test all platforms separately
- **Complexity**: More moving parts to manage

#### Best For
- When you have resources for separate implementations
- When platform-specific optimization is critical
- Mature products with dedicated mobile and web teams

---

## 5. Comparative Analysis

### Performance Comparison

| Solution | iOS/Android | Web | FPS | Bundle Size |
|----------|-------------|-----|-----|-------------|
| React Native Skia | Excellent | Excellent | 60-120 | Medium |
| Fabric.js via WebView | Fair | Excellent | 30-60 | Large (WebView) |
| Konva via WebView | Fair | Excellent | 30-60 | Large (WebView) |
| Polotno via WebView | Fair | Excellent | 30-60 | Large |
| react-native-svg | Good | Good | 30-60 | Small |

### Feature Completeness

| Solution | Drag | Resize | Rotate | Text Edit | Export | Undo/Redo |
|----------|------|--------|--------|-----------|--------|-----------|
| React Native Skia | ✅ (custom) | ✅ (custom) | ✅ (custom) | ✅ (custom) | ✅ | ❌ (custom) |
| Fabric.js | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Konva | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ (custom) |
| Polotno | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| react-native-svg | ⚠️ (custom) | ⚠️ (custom) | ⚠️ (custom) | ❌ | ⚠️ | ❌ |

### Maintenance & Community

| Solution | GitHub Stars | Last Update | Active | License | Cost |
|----------|-------------|-------------|--------|---------|------|
| React Native Skia | ~8k | Nov 2025 | ✅ Very Active | MIT | Free |
| Fabric.js | ~30k | 2025 | ✅ Very Active | MIT | Free |
| Konva | ~13.8k | Nov 2025 | ✅ Very Active | MIT | Free |
| Polotno | N/A (Commercial) | 2025 | ✅ Active | Commercial | $155+/mo |
| react-native-svg | ~7.2k | 2025 | ✅ Very Active | MIT | Free |

### Implementation Complexity

| Solution | Setup | Custom UI | Learning Curve | Time to MVP |
|----------|-------|-----------|----------------|-------------|
| React Native Skia | Medium | High (all custom) | Medium | 2-4 weeks |
| Polotno WebView | Easy | Low (pre-built) | Low | 1 week |
| Fabric.js WebView | Medium | Medium | Medium | 2 weeks |
| Platform-Specific | High | High | High | 4-6 weeks |

---

## 6. Final Recommendations

### For MeetMeAt App: React Native Skia (Primary) + Strategic Approach

#### Why React Native Skia is the Best Choice

1. **True Cross-Platform**: Same codebase for iOS, Android, and Web with native performance on all platforms
2. **Native Feel**: Runs on UI thread, 60-120 FPS, haptic feedback, smooth gestures
3. **No Licensing Costs**: MIT license, no recurring fees
4. **Future-Proof**: Backed by Shopify, active development, growing ecosystem
5. **Full Control**: Build exactly the features you need, no more, no less
6. **Production-Ready**: Used by major apps (Shopify, Notesnook, others)
7. **Export Built-In**: makeImageSnapshot for PNG/JPG export

#### Implementation Roadmap

**Phase 1: MVP (2-3 weeks)**
- Set up Skia canvas with basic rendering
- Implement gesture handling for drag
- Add image and text elements
- Basic export to PNG

**Phase 2: Core Features (2-3 weeks)**
- Selection system with visual feedback
- Resize handles and pinch-to-scale
- Rotation with two-finger gesture
- Color picker for elements
- Inline text editing

**Phase 3: Polish (1-2 weeks)**
- Undo/redo functionality
- Element layering (bring to front/send to back)
- Templates and presets
- Performance optimization

**Phase 4: Advanced (optional)**
- Filters and effects
- Stickers and graphics library
- Collaborative editing
- Cloud save/sync

#### Technical Architecture

```
MeetMeAt Poster Editor
├── UI Layer (React Native)
│   ├── Toolbar (React Native components)
│   ├── Property Panel (color, size, etc.)
│   └── Export/Share buttons
│
├── Editor Layer (Skia Canvas)
│   ├── Canvas Component
│   ├── Element Renderer (images, text, shapes)
│   ├── Selection Overlay
│   └── Transform Handles
│
├── Gesture Layer (Gesture Handler + Reanimated)
│   ├── Pan Gesture (drag)
│   ├── Pinch Gesture (scale)
│   ├── Rotation Gesture
│   └── Tap Gesture (selection)
│
└── State Management
    ├── Poster State (elements array)
    ├── Selection State (active element)
    ├── History State (undo/redo)
    └── Export State (save/share)
```

#### Key Dependencies

```json
{
  "dependencies": {
    "@shopify/react-native-skia": "^2.0.0",
    "react-native-gesture-handler": "^2.14.0",
    "react-native-reanimated": "^3.6.0",
    "react-native-fs": "^2.20.0",
    "react-native-share": "^10.0.0"
  }
}
```

### Alternative: Polotno WebView (If Budget Allows)

**Choose this if:**
- You need an MVP in 1 week
- Budget allows $155/month
- Web-first approach is acceptable
- Don't mind slightly less native feel

**Implementation:**
- Embed Polotno Studio in WebView for mobile
- Use Polotno SDK directly for web version
- Bridge communication via postMessage

---

## 7. Risks & Considerations

### React Native Skia Approach

**Risks:**
- Development time longer than WebView approach
- Need to build UI components from scratch
- Learning curve for team unfamiliar with Skia

**Mitigations:**
- Start with basic features, iterate
- Use react-native-free-canvas as reference
- Leverage existing examples from Notesnook and others
- Consider hiring contractor with Skia experience for initial setup

### WebView Hybrid Approach

**Risks:**
- Performance on lower-end devices
- WebView memory usage
- Communication bridge complexity
- Less native feel

**Mitigations:**
- Optimize WebView content
- Lazy load assets
- Implement proper loading states
- Test extensively on target devices

### Platform-Specific Approach

**Risks:**
- 2x maintenance burden
- Feature parity drift
- Testing overhead

**Mitigations:**
- Only consider if resources allow
- Share state management logic
- Use feature flags for platform differences

---

## 8. Example Projects to Study

### React Native Skia Examples
1. **Notesnook Drawing App**: https://blog.notesnook.com/drawing-app-with-react-native-skia/
   - 60 FPS freehand drawing
   - Path selection and manipulation
   - Export functionality

2. **react-native-free-canvas**: https://github.com/doublelam/react-native-free-canvas
   - Zoom and pan implementation
   - Gesture handling patterns
   - Performance optimizations

3. **SolankiYogesh/SkiaAnimations**: https://github.com/SolankiYogesh/SkiaAnimations
   - Animation examples with Skia
   - Beautiful visual effects

### Fabric.js Examples (Web Reference)
1. **fabric-with-gestures Demo**: Mobile touch and pinch zoom
2. **Sandro Turriate's Pinch-to-Zoom**: https://turriate.com/articles/how-to-pinch-to-zoom-2-finger-pan-fabricjs-canvas

### Complete Editors (Web)
1. **Polotno Studio**: https://studio.polotno.com/ (live demo)
2. **ajpgtech/design-editor**: Open source Canva clone

---

## 9. Decision Matrix

| Criteria | Weight | React Native Skia | Polotno WebView | Platform-Specific |
|----------|--------|-------------------|-----------------|-------------------|
| Native Performance | 25% | 9/10 | 6/10 | 9/10 |
| Cross-Platform | 20% | 10/10 | 8/10 | 7/10 |
| Development Speed | 15% | 6/10 | 10/10 | 4/10 |
| Maintenance | 15% | 8/10 | 7/10 | 5/10 |
| Cost | 10% | 10/10 | 6/10 | 8/10 |
| Flexibility | 10% | 10/10 | 6/10 | 9/10 |
| Community Support | 5% | 8/10 | 7/10 | 7/10 |
| **Total Score** | | **8.45** | **7.25** | **7.05** |

**Winner: React Native Skia** with significant lead in long-term value.

---

## 10. Next Steps

### Immediate Actions

1. **Set up proof-of-concept** (2-3 days)
   - Install react-native-skia, gesture-handler, reanimated
   - Create basic canvas with one draggable image
   - Test export to PNG functionality

2. **Evaluate POC** (1 day)
   - Assess performance on real devices
   - Check web compatibility
   - Measure development velocity

3. **Make final decision** (1 day)
   - If POC succeeds → proceed with Skia
   - If POC struggles → consider Polotno WebView fallback
   - Document decision and rationale

### Learning Resources

**React Native Skia:**
- Official Docs: https://shopify.github.io/react-native-skia/
- Tutorial: "Building a Hand Drawing App": https://medium.com/react-native-rocket/building-a-hand-drawing-app-with-react-native-skia-and-gesture-handler-9797f5f7b9b4
- Shopify Blog: "Getting Started with React Native Skia"

**Gesture Handler:**
- Official Docs: https://docs.swmansion.com/react-native-gesture-handler/
- Gesture Composition Guide

**Reanimated:**
- Official Docs: https://docs.swmansion.com/react-native-reanimated/

---

## Summary

**Primary Recommendation:** React Native Skia + Gesture Handler + Reanimated
- Best long-term choice for native cross-platform poster editor
- Native performance, full control, no licensing costs
- 2-4 week MVP development time
- Production-ready and future-proof

**Fallback Option:** Polotno SDK in WebView
- Fastest to MVP (1 week)
- Requires $155+/month licensing
- Good for validation/prototype
- Can migrate to Skia later if needed

**Not Recommended:**
- Fabric.js (web-only, no native RN support)
- Konva (web-only, no native RN support)
- PixiJS (game-focused, complex RN integration)
- react-native-canvas (deprecated, poor performance)

---

## Sources

### React Native Skia
- [Canvas | React Native Skia](https://shopify.github.io/react-native-skia/docs/canvas/overview/)
- [Getting Started with React Native Skia - Shopify](https://shopify.engineering/getting-started-with-react-native-skia)
- [Using React Native Skia to Build a 60 FPS Free-hand Drawing App](https://blog.notesnook.com/drawing-app-with-react-native-skia/)
- [Using React Native Skia for Web Graphics with Expo 2026](https://dev.to/sherry_walker_bba406fb339/using-react-native-skia-for-web-graphics-with-expo-2026-3chj)
- [Images | React Native Skia](https://shopify.github.io/react-native-skia/docs/images/)
- [Snapshot Views | React Native Skia](https://shopify.github.io/react-native-skia/docs/snapshotviews/)

### React Native Canvas Libraries
- [GitHub - terrylinla/react-native-sketch-canvas](https://github.com/terrylinla/react-native-sketch-canvas)
- [react-native-canvas - npm](https://www.npmjs.com/package/react-native-canvas)
- [GitHub - doublelam/react-native-free-canvas](https://github.com/doublelam/react-native-free-canvas)
- [GitHub - BenJeau/react-native-draw](https://github.com/BenJeau/react-native-draw)

### Fabric.js
- [GitHub - fabricjs/fabric.js](https://github.com/fabricjs/fabric.js)
- [Fabric.js Javascript Library](https://fabricjs.com/)
- [Releases · fabricjs/fabric.js](https://github.com/fabricjs/fabric.js/releases)
- [Performant Drag and Zoom using Fabric.js](https://medium.com/@Fjonan/performant-drag-and-zoom-using-fabric-js-3f320492f24b)
- [Add mobile gestures in Fabric.js](https://sketchmate.ninja/blog/posts/pinch/)
- [How to pinch-to-zoom and 2 finger pan a Fabric.js canvas](https://turriate.com/articles/how-to-pinch-to-zoom-2-finger-pan-fabricjs-canvas)

### Konva.js
- [GitHub - konvajs/konva](https://github.com/konvajs/konva)
- [Getting started with React and Canvas via Konva](https://konvajs.org/docs/react/index.html)
- [react-konva - npm](https://www.npmjs.com/package/react-konva)
- [GitHub - konvajs/react-konva](https://github.com/konvajs/react-konva)

### Polotno
- [Polotno – Your powerful visual design suite](http://www.polotno.dev/)
- [GitHub - polotno-project/polotno-studio](https://github.com/polotno-project/polotno-studio)
- [Overview | Polotno SDK Documentation](https://polotno.com/docs/overview)
- [Try Polotno SDK free – flexible pricing](https://polotno.com/sdk/pricing)
- [IMG.LY vs Polotno Alternative](https://img.ly/polotno-alternative)

### PixiJS
- [Introducing PixiJS React v8](https://pixijs.com/blog/pixi-react-v8-live)
- [expo-pixi - npm](https://www.npmjs.com/package/expo-pixi)
- [GitHub - flyskywhy/react-native-pixi](https://github.com/flyskywhy/react-native-pixi)
- [PixiJS React | PixiJS React](https://react.pixijs.io/)

### Open-Source Canva Clones
- [GitHub - imgly/canva-clone-react-cesdk](https://github.com/imgly/canva-clone-react-cesdk)
- [canva-clone · GitHub Topics](https://github.com/topics/canva-clone)
- [GitHub - ajpgtech/design-editor](https://github.com/ajpgtech/design-editor)
- [GitHub - designcombo/react-video-editor](https://github.com/designcombo/react-video-editor)

### WebView Hybrid Approach
- [react-native-canvas - npm](https://www.npmjs.com/package/react-native-canvas)
- [GitHub - nora-soderlund/react-native-webview-canvas](https://github.com/nora-soderlund/react-native-webview-canvas)
- [React Native WebView: A complete guide](https://blog.logrocket.com/react-native-webview-complete-guide/)
- [Creating Hybrid Mobile Apps with React Native and WebView](https://andreadams.com.br/creating-hybrid-mobile-apps-with-react-native-and-webview-a-comprehensive-guide/)

### Gesture Handling
- [Add gestures - Expo Documentation](https://docs.expo.dev/tutorial/gestures/)
- [React Native Gesture Handler](https://docs.swmansion.com/react-native-gesture-handler/)
- [Gesture composition & interactions](https://docs.swmansion.com/react-native-gesture-handler/docs/fundamentals/gesture-composition/)
- [Gestures | React Native Skia](https://shopify.github.io/react-native-skia/docs/animations/gestures/)

### Expo Examples
- [GitHub - thomas-coldwell/expo-image-editor](https://github.com/thomas-coldwell/expo-image-editor)
- [GitHub - Hokyjack/expo-perfect-canvas](https://github.com/Hokyjack/expo-perfect-canvas)
- [GitHub - MarangoniEduardo/expo-draw](https://github.com/MarangoniEduardo/expo-draw)

### React Native SVG
- [Draggable SVG elements](https://www.petercollingridge.co.uk/tutorials/svg/interactive/dragging/)
- [GitHub issue #1498 - react-native-svg](https://github.com/software-mansion/react-native-svg/issues/1498)

---

**Research completed by:** Claude (Anthropic)
**Date:** December 8, 2025
**For:** MeetMeAt Mobile App - Poster Editor Feature
