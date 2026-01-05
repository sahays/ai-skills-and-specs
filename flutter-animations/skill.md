---
name: flutter-animations
description: Implement animations in Flutter using implicit, explicit, and physics-based approaches. Use when adding motion, transitions, or interactive animations to UI.
---

- Core Principles
  - Implicit first: use AnimatedFoo widgets (AnimatedContainer, AnimatedOpacity) for simple property animations, less code, automatic animation
  - Explicit for control: use AnimationController when timing, sequencing, or custom curves needed
  - Physics-based for realism: use springs and friction for natural motion (BouncingScrollPhysics, SpringSimulation)
  - 60 FPS target: keep animations smooth, avoid heavy computation in animation callbacks

- Implicit Animations
  - Built-in widgets: AnimatedContainer, AnimatedOpacity, AnimatedPositioned, AnimatedAlign, AnimatedPadding, AnimatedSize
  - Pattern: change property value, widget animates automatically over specified duration
  - Custom implicit: use TweenAnimationBuilder for animating any custom property
  - Duration and curves: set duration and curve parameters for timing and easing

- Explicit Animations
  - Setup requirements: AnimationController + Animation<T> + AnimatedBuilder or AnimatedWidget
  - Ticker provider: use SingleTickerProviderStateMixin (one controller) or TickerProviderStateMixin (multiple)
  - Lifecycle: initialize in initState, dispose in dispose to prevent memory leaks
  - Control methods: use forward(), reverse(), repeat(), stop() to control animation playback
  - Listen for updates: use AnimatedBuilder to rebuild only animated portions of widget tree

- Animation Curves
  - Standard: Curves.easeInOut for most transitions
  - Entrances: Curves.easeOut for elements appearing
  - Exits: Curves.easeIn for elements disappearing
  - Bouncy: Curves.elasticOut, Curves.bounceOut for playful motion
  - Custom: define Cubic curves for brand-specific motion design

- Staggered Animations
  - Sequential timing: use Interval curve to offset multiple animations on single controller
  - Coordinated motion: animate multiple properties with different timing using same controller
  - Overlap control: adjust interval start/end values (0.0 to 1.0) to control overlap

- Hero Animations
  - Shared element transitions: wrap widget in Hero with matching tag on both screens
  - Automatic flight: Flutter automatically animates position, size, and shape between routes
  - Tag uniqueness: ensure tag is unique within each route but identical across routes
  - Custom flight: override createRectTween for custom path curves

- Page Transitions
  - Custom routes: use PageRouteBuilder with custom transitionsBuilder
  - Built-in transitions: SlideTransition, FadeTransition, ScaleTransition, RotationTransition
  - Combine transitions: layer multiple transition widgets for complex effects
  - Secondary animation: use secondaryAnimation for exit transitions of previous route

- Performance
  - Repaint boundaries: wrap animated widgets with RepaintBoundary to isolate repaints
  - AnimatedBuilder: rebuild only animated subtree, not entire widget tree
  - Avoid setState: use AnimatedBuilder instead of setState in animation listeners
  - Dispose controllers: always dispose controllers in dispose() to prevent memory leaks
  - Reduce layers: minimize Opacity widget (expensive), prefer AnimatedOpacity

- Common Patterns
  - Fade in on mount: AnimatedOpacity with delayed setState or TweenAnimationBuilder
  - Slide from edge: AnimatedPositioned or SlideTransition with Offset tween
  - Expand/collapse: AnimatedSize or SizeTransition for height/width changes
  - Loading indicators: CircularProgressIndicator or custom with RotationTransition
  - Shimmer effect: gradient animation with repeating AnimationController
  - Pull to refresh: RefreshIndicator with physics-based spring animation

- Accessibility
  - Reduce motion: check MediaQuery.of(context).disableAnimations and reduce/skip decorative animations
  - Duration limits: keep essential animations under 500ms for better accessibility
  - Optional animations: allow users to disable non-essential animations in settings
  - Maintain functionality: ensure animations don't prevent access to content or features
