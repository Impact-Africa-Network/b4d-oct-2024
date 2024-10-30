### Template

Visual Analysis:
1. Typography
- Heading: [specify font family, weight, size, line height, color]
- Body text: [specify font family, weight, size, line height, color]
- Buttons/CTAs: [specify font family, weight, size, color]

2. Layout & Spacing
- Container max-width: [specify]
- Padding/margins: [specify scale e.g., 16px/24px/32px/48px/64px]
- Grid system: [specify columns, gaps]
- Section spacing: [specify vertical rhythm]

3. Color Palette
- Primary: [specify hex/rgb]
- Secondary: [specify hex/rgb]
- Text colors: [specify hex/rgb]
- Background colors: [specify hex/rgb]
- Accent colors: [specify hex/rgb]

4. Component Details
Header/Hero:
- Exact layout specifications
- Text alignment
- Spacing between elements
- Button styles & states

Main Content:
- Grid/flex specifications
- Card/item dimensions
- Image treatment (aspect ratio, object-fit)
- Spacing between sections

5. Interactive Elements & Animations
Hover States:
- Scale/transform values
- Transition timing (e.g., 0.2s ease)
- Color changes
- Shadow adjustments

Click/Active States:
- Transform properties
- Color shifts
- Feedback animations

Page Animations:
- Entry animations
- Scroll behavior
- Loading states

6. Responsive Behavior
Breakpoints:
- Mobile: < 768px
- Tablet: 768px - 1024px
- Large Mobile Phones - 960px-1024px
- Desktop: > 1024px
- 

Responsive Rules:
- Typography scale adjustments
- Layout changes at each breakpoint
- Spacing modifications
- Component reflow patterns
- Image sizing/cropping rules

7. Performance Requirements
- Image optimization
- Animation performance
- Loading strategy
- CSS organization

Technical Requirements:
1. Use semantic HTML5
2. Implement modern CSS:
   - CSS Grid/Flexbox
   - Custom properties (variables)
   - clamp() for fluid typography
   - calc() for dynamic spacing
   - aspect-ratio
   - container queries if needed

3. JavaScript (if required):
   - Minimal usage
   - Performance-focused
   - Progressive enhancement
   - Smooth scroll behavior
   - Intersection Observer for animations

4. Cross-browser compatibility:
   - Latest 2 versions of major browsers
   - Graceful degradation strategy

Please provide the complete implementation with:
- Organized, commented HTML
- Modular CSS with clear naming conventions
- Necessary JavaScript with performance considerations
- Responsive design implementation
- Animation definitions
- Loading/performance optimizations



### Prompt 
I am attaching a section from my landing page design in Figma. I need a pixel-perfect implementation of this design with:

Typography:
- Heading: system-ui, 'Raleway', -apple-system, sans-serif | 48px (clamp: 2.5rem, 5vw, 4rem) | #1a1a1a
- Body: system-ui, 'Raleway', -apple-system, sans-serif | 18px (clamp: 1rem, 2vw, 1.2rem) | #666
- Button: system-ui, 'Raleway', -apple-system, sans-serif | 18px | #fff

Layout:
- Hero: centered, max-width 800px
- Gallery: max-width 1400px, 4x2 grid
- Gaps: 16px consistent
- Section spacing: 64px between hero and gallery

Components:
Gallery Items:
- 1:1 aspect ratio
- 16px border radius
- Scale transform on hover (1.02)
- Smooth transition (0.3s ease)

Animations:
- Hover: scale(1.02) transition
- Button: translateY(-2px) on hover
- Page load: [optional] fade-in animation

Responsive:
Mobile (<768px):
- Single column gallery
- Reduced padding (16px)
- Adjusted typography scale
- Preserved image aspect ratios

Colors:
- Background: #fff
- Text: #1a1a1a (heading), #666 (body)
- Button: #1a1a1a (normal), #333 (hover)

Performance:
- Optimize images
- Minimal JS (only for smooth scroll)
- CSS organized by component
- Responsive images