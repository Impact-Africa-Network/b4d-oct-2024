### Approach

- 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Discover Beauty Around the World</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Raleway:ital,wght@0,100..900;1,100..900&family=Roboto+Mono:ital,wght@0,100..700;1,100..700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            line-height: 1.5;
            color: #333;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 24px;
        }

        .hero {
            text-align: center;
            padding: clamp(48px, 10vw, 96px) 24px;
        }

        .hero h1 {
            font-size: clamp(32px, 5vw, 48px);
            font-weight: 700;
            margin-bottom: 24px;
            line-height: 1.2;
        }

        .hero p {
            color: #666;
            max-width: 600px;
            margin: 0 auto 32px;
            font-size: clamp(16px, 2vw, 18px);
        }

        .button {
            display: inline-block;
            padding: 16px 32px;
            background: #000;
            color: white;
            text-decoration: none;
            border-radius: 32px;
            font-weight: 500;
            transition: transform 0.2s ease, background-color 0.2s ease;
        }

        .button:hover {
            transform: translateY(-2px);
            background: #222;
        }

        .gallery {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 16px;
            padding: 0 24px 64px;
        }

        .gallery-item {
            position: relative;
            border-radius: 16px;
            overflow: hidden;
            aspect-ratio: 1;
            transition: transform 0.3s ease;
        }

        .gallery-item:hover {
            transform: scale(1.03);
        }

        .gallery-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        @media (max-width: 1024px) {
            .gallery {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        @media (max-width: 768px) {
            .gallery {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <main>
        <section class="hero">
            <div class="container">
                <h1>Discover the beauty<br>around the world</h1>
                <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Faucibus in libero risus semper habitant arcu eget. Et integer facilisi eget diam.</p>
                <a href="#" class="button">Explore</a>
            </div>
        </section>

        <div class="container">
            <div class="gallery">
                <div class="gallery-item">
                    <img src="https://img.freepik.com/free-photo/abstract-colorful-splash-3d-background-generative-ai-background_60438-2509.jpg" alt="Abstract orange spiral pattern">
                </div>
                <div class="gallery-item">
                    <img src="https://as2.ftcdn.net/v2/jpg/09/05/25/93/1000_F_905259335_m7nsPKDmnxsj6gWwUJdxVwTIG6MAUcUy.jpg" alt="Ethereal sphere with pastel colors">
                </div>
                <div class="gallery-item">
                    <img src="https://img.freepik.com/free-vector/speed-motion-background-with-fast-speedometer-car-racing-velocity-background_60438-2046.jpg" alt="Geometric shapes in warm tones">
                </div>
                <div class="gallery-item">
                    <img src="https://img.freepik.com/free-photo/abstract-backdrop-pattern-illustration-decoration-design-curve-shape-generative-ai_188544-12865.jpg" alt="Abstract purple fluid shapes">
                </div>
                <div class="gallery-item">
                    <img src="https://img.freepik.com/free-photo/colorful-painting-with-word-apple-it_1340-35698.jpg" alt="Gradient with red and purple">
                </div>
                <div class="gallery-item">
                    <img src="https://img.freepik.com/free-photo/geometric-seamless-pattern_23-2151021448.jpg" alt="Warm orange and pink gradient">
                </div>
                <div class="gallery-item">
                    <img src="https://img.freepik.com/free-photo/abstract-colorful-splash-3d-background-generative-ai-background_60438-2503.jpg" alt="3D floating circles">
                </div>
                <div class="gallery-item">
                    <img src="https://img.freepik.com/free-photo/wooden-shelf-with-pattern-that-says-word-it_1340-28977.jpg" alt="Blue bubbles pattern">
                </div>
            </div>
        </div>
    </main>
</body>
</html>
```