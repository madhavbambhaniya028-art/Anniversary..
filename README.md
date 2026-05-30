# Anniversary..
3 years 
<?php
// --- CONFIGURATION ---
// Set your actual wedding date here (YYYY-MM-DD format)
$wedding_date = "2023-05-31"; 
$years_together = 3;

// Handle the secret message reveal
$reveal_message = false;
if (isset($_POST['reveal'])) {
    $reveal_message = true;
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy 3rd Anniversary!</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Great+Vibes&family=Poppins:wght@300;400;600&display=swap');

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(135deg, #ffe5ec, #ffc2d1, #ffb3c6);
            background-size: 400% 400%;
            animation: gradientBG 15s ease infinite;
            font-family: 'Poppins', sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow-x: hidden;
            perspective: 1000px;
        }

        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        /* Background Canvas for Particles */
        #heartCanvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            pointer-events: none;
        }

        /* Main Card Container with Glassmorphism */
        .card {
            background: rgba(255, 255, 255, 0.45);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.25);
            padding: 3rem 2rem;
            border-radius: 24px;
            width: 90%;
            max-width: 550px;
            text-align: center;
            box-shadow: 0 8px 32px 0 rgba(255, 75, 110, 0.2);
            z-index: 2;
            transform: translateY(30px);
            opacity: 0;
            animation: slideUp 1.2s cubic-bezier(0.4, 0, 0.2, 1) forwards;
        }

        @keyframes slideUp {
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        h1 {
            font-family: 'Great Vibes', cursive;
            font-size: 3.5rem;
            color: #ff477e;
            margin-bottom: 0.5rem;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.05);
            animation: pulse 2s infinite alternate;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.03); }
        }

        .subtitle {
            font-size: 1.1rem;
            color: #6c757d;
            font-weight: 300;
            margin-bottom: 2rem;
            letter-spacing: 1px;
        }

        /* Counter Styling */
        .counter-container {
            display: flex;
            justify-content: space-around;
            margin-bottom: 2.5rem;
            background: rgba(255, 255, 255, 0.3);
            padding: 1rem;
            border-radius: 16px;
        }

        .counter-item {
            display: flex;
            flex-direction: column;
        }

        .counter-number {
            font-size: 1.8rem;
            font-weight: 600;
            color: #ff477e;
        }

        .counter-label {
            font-size: 0.75rem;
            text-transform: uppercase;
            color: #6c757d;
            letter-spacing: 1px;
            margin-top: 2px;
        }

        /* Button and Form */
        .btn {
            background: linear-gradient(45deg, #ff477e, #ff7096);
            color: white;
            border: none;
            padding: 0.8rem 2rem;
            font-size: 1rem;
            font-weight: 600;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(255, 75, 110, 0.4);
            transition: all 0.3s ease;
            text-decoration: none;
            display: inline-block;
        }

        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(255, 75, 110, 0.6);
        }

        .btn:active {
            transform: translateY(-1px);
        }

        /* Animated Message Box */
        .message-box {
            margin-top: 2rem;
            padding: 1.5rem;
            background: rgba(255, 255, 255, 0.7);
            border-left: 4px solid #ff477e;
            border-radius: 8px;
            font-style: italic;
            color: #495057;
            line-height: 1.6;
            animation: fadeIn 1s ease forwards;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.95); }
            to { opacity: 1; transform: scale(1); }
        }
    </style>
</head>
<body>

    <canvas id="heartCanvas"></canvas>

    <div class="card">
        <h1>Happy 3rd Anniversary!</h1>
        <p class="subtitle">3 Years of Love, Laughter & Beautiful Memories</p>

        <div class="counter-container">
            <div class="counter-item">
                <span class="counter-number" id="days">00</span>
                <span class="counter-label">Days</span>
            </div>
            <div class="counter-item">
                <span class="counter-number" id="hours">00</span>
                <span class="counter-label">Hours</span>
            </div>
            <div class="counter-item">
                <span class="counter-number" id="minutes">00</span>
                <span class="counter-label">Mins</span>
            </div>
            <div class="counter-item">
                <span class="counter-number" id="seconds">00</span>
                <span class="counter-label">Secs</span>
            </div>
        </div>

        <?php if (!$reveal_message): ?>
            <form method="POST" action="">
                <button type="submit" name="reveal" class="btn">Click to Unlock My Message</button>
            </form>
        <?php else: ?>
            <div class="message-box">
                "Three years down, a lifetime to go. Every single day with you reminds me how incredibly lucky I am. Thank you for being my partner, my best friend, and my greatest adventure. Happy Anniversary, my love! ❤️"
            </div>
        <?php endif; ?>
    </div>

    <script>
        // --- COUNT-UP TIMER LOGIC ---
        // Pass PHP date directly into JavaScript safely
        const weddingDate = new Date("<?php echo $wedding_date; ?>T00:00:00").getTime();

        function updateCounter() {
            const now = new Date().getTime();
            const difference = now - weddingDate;

            // Time calculations for days, hours, minutes and seconds
            const days = Math.floor(difference / (1000 * 60 * 60 * 24));
            const hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((difference % (1000 * 60)) / 1000);

            // Output the result in the elements
            document.getElementById("days").innerText = days.toString().padStart(2, '0');
            document.getElementById("hours").innerText = hours.toString().padStart(2, '0');
            document.getElementById("minutes").innerText = minutes.toString().padStart(2, '0');
            document.getElementById("seconds").innerText = seconds.toString().padStart(2, '0');
        }

        // Update the count down every 1 second
        setInterval(updateCounter, 1000);
        updateCounter(); // Run immediately


        // --- FLOATING HEARTS CANVAS ANIMATION ---
        const canvas = document.getElementById('heartCanvas');
        const ctx = canvas.getContext('2d');

        let particles = [];

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        class HeartParticle {
            constructor() {
                this.x = Math.random() * canvas.width;
                this.y = canvas.height + Math.random() * 50;
                this.size = Math.random() * 15 + 10;
                this.speedY = Math.random() * 1.5 + 0.5;
                this.speedX = Math.sin(Math.random() * 2) * 0.5;
                this.opacity = Math.random() * 0.6 + 0.2;
            }

            draw() {
                ctx.save();
                ctx.globalAlpha = this.opacity;
                ctx.fillStyle = '#ff477e';
                ctx.beginPath();
                
                // Drawing a heart shape using Bezier curves
                let topCurveHeight = this.size * 0.3;
                ctx.moveTo(this.x, this.y + topCurveHeight);
                ctx.bezierCurveTo(this.x, this.y, this.x - this.size / 2, this.y, this.x - this.size / 2, this.y + topCurveHeight);
                ctx.bezierCurveTo(this.x - this.size / 2, this.y + (this.size + topCurveHeight) / 2, this.x, this.y + (this.size + topCurveHeight) / 2, this.x, this.y + this.size);
                ctx.bezierCurveTo(this.x, this.y + (this.size + topCurveHeight) / 2, this.x + this.size / 2, this.y + (this.size + topCurveHeight) / 2, this.x + this.size / 2, this.y + topCurveHeight);
                ctx.bezierCurveTo(this.x + this.size / 2, this.y, this.x, this.y, this.x, this.y + topCurveHeight);
                
                ctx.closePath();
                ctx.fill();
                ctx.restore();
            }

            update() {
                this.y -= this.speedY;
                this.x += this.speedX;
                if (this.y + this.size < 0) {
                    this.y = canvas.height + Math.random() * 50;
                    this.x = Math.random() * canvas.width;
                }
            }
        }

        function initParticles() {
            particles = [];
            const particleCount = Math.min(60, Math.floor(canvas.width / 20));
            for (let i = 0; i < particleCount; i++) {
                particles.push(new HeartParticle());
            }
        }

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            particles.forEach(p => {
                p.update();
                p.draw();
            });
            requestAnimationFrame(animate);
        }

        initParticles();
        animate();
    </script>
</body>
</html>
