document.addEventListener('DOMContentLoaded', function() {
    // Canvas setup
    const canvas = document.getElementById('starfield');
    const ctx = canvas.getContext('2d');

    // Set canvas size
    function setCanvasSize() {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
    }
    setCanvasSize();
    window.addEventListener('resize', setCanvasSize);

    // Star properties
    class Star {
        constructor() {
            this.reset();
            this.y = Math.random() * canvas.height; // Initial random position
        }

        reset() {
            this.x = Math.random() * canvas.width;
            this.y = 0;
            this.size = Math.random() * 3;
            this.speed = Math.random() * 3 + 1;
            this.opacity = Math.random();
            this.twinkleSpeed = Math.random() * 0.05;
        }

        update() {
            this.y += this.speed;
            this.opacity += Math.sin(Date.now() * this.twinkleSpeed) * 0.01;
            
            // Keep opacity between 0.3 and 1
            this.opacity = Math.max(0.3, Math.min(1, this.opacity));

            if (this.y > canvas.height) {
                this.reset();
            }
        }

        draw() {
            ctx.beginPath();
            ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
            ctx.fillStyle = `rgba(255, 255, 255, ${this.opacity})`;
            ctx.fill();
        }
    }

    // Create stars
    const stars = Array(300).fill().map(() => new Star());

    // Animation function
    function animate() {
        // Clear canvas with semi-transparent black for trail effect
        ctx.fillStyle = 'rgba(0, 0, 0, 0.1)';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        // Update and draw stars
        stars.forEach(star => {
            star.update();
            star.draw();
        });

        // Add glow effect to some stars
        stars.forEach(star => {
            if (star.size > 2) {
                ctx.beginPath();
                const gradient = ctx.createRadialGradient(
                    star.x, star.y, 0,
                    star.x, star.y, star.size * 4
                );
                gradient.addColorStop(0, 'rgba(255, 255, 255, 0.3)');
                gradient.addColorStop(1, 'rgba(255, 255, 255, 0)');
                ctx.fillStyle = gradient;
                ctx.arc(star.x, star.y, star.size * 4, 0, Math.PI * 2);
                ctx.fill();
            }
        });

        requestAnimationFrame(animate);
    }

    // Start animation
    animate();

    // Add mouse interaction
    let mouseX = 0;
    let mouseY = 0;
    let mouseSpeed = 0;

    canvas.addEventListener('mousemove', (e) => {
        const lastX = mouseX;
        const lastY = mouseY;
        mouseX = e.clientX;
        mouseY = e.clientY;
        
        // Calculate mouse speed
        const dx = mouseX - lastX;
        const dy = mouseY - lastY;
        mouseSpeed = Math.sqrt(dx * dx + dy * dy);

        // Create new stars based on mouse speed
        if (mouseSpeed > 20) {
            const newStar = new Star();
            newStar.x = mouseX;
            newStar.y = mouseY;
            newStar.size = Math.random() * 2 + 1;
            stars.push(newStar);
            
            // Keep array size manageable
            if (stars.length > 400) {
                stars.shift();
            }
        }
    });
});
