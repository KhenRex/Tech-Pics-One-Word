<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>4 Pics 1 Word - Futuristic VIP Edition</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600&family=Rajdhani:wght@700&display=swap" rel="stylesheet">
    <style>
        /* LOADING ANIMATION STYLES */
        .background {
            position: absolute;
            width: 100%;
            height: 100%;
            background: linear-gradient(45deg, #001122, #003366, #001122);
            background-size: 400% 400%;
            animation: bgAnimation 5s infinite alternate ease-in-out;
            z-index: -1;
        }

        @keyframes bgAnimation {
            0% { background-position: 0% 50%; }
            100% { background-position: 100% 50%; }
        }

        .loading-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 100%;
            height: 100vh;
            position: absolute;
            top: 0;
            left: 0;
            z-index: 1000;
        }

        .word-tiles {
            display: flex;
            gap: 8px;
        }

        .tile {
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: bold;
            background: linear-gradient(135deg, #00ffee, #0088ff);
            border-radius: 12px;
            box-shadow: 0 0 20px rgba(0, 255, 238, 0.8);
            opacity: 0;
            transform: scale(0.8);
            animation: tileFadeIn 1s ease-in-out forwards;
            position: relative;
            overflow: hidden;
        }

        @keyframes tileFadeIn {
            0% { opacity: 0; transform: scale(0.8); }
            100% { opacity: 1; transform: scale(1); }
        }

        .tile::after {
            content: "";
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            background: radial-gradient(circle, rgba(255, 255, 255, 0.3), transparent);
            opacity: 0;
            animation: waveShine 2s infinite alternate ease-in-out;
        }

        @keyframes waveShine {
            0% { opacity: 0.3; transform: scale(1); }
            100% { opacity: 0.7; transform: scale(1.1); }
        }

        .start-btn {
            margin-top: 20px;
            padding: 16px 40px;
            font-size: 22px;
            font-weight: bold;
            color: white;
            background: linear-gradient(135deg, #00ffee, #0088ff);
            border: none;
            border-radius: 30px;
            cursor: pointer;
            transition: 0.3s;
            box-shadow: 0 0 20px #00ffee;
            display: none;
            animation: fadeIn 1s ease-in-out;
        }

        .start-btn:hover {
            box-shadow: 0 0 30px #00ffee;
            transform: scale(1.1);
        }

        .countdown {
            font-size: 40px;
            font-weight: bold;
            color: #00ffee;
            text-shadow: 0 0 15px rgba(0, 255, 238, 0.8);
            display: none;
            animation: fadeIn 1s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.9); }
            to { opacity: 1; transform: scale(1); }
        }

        /* ORIGINAL GAME STYLES */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
            color: #fff;
            overflow: hidden;
            user-select: none;
            transition: background 2s ease-in-out;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        #gameContainer {
            display: none;
            padding: 20px;
            max-width: 1200px;
            margin: auto;
            text-align: center;
            animation: fadeIn 1s ease-in-out;
        }

        .images {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
            margin: 20px 0;
        }

        .images img {
            width: 200px;
            height: 200px;
            border-radius: 15px;
            object-fit: cover;
            box-shadow: 0 8px 20px rgba(0,0,0,0.4);
        }

        #result {
            margin-top: 20px;
        }

        #popup {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            animation: fadeIn 0.5s ease-in-out;
        }

        #popupContent {
            background: #1d4350;
            padding: 20px;
            border-radius: 20px;
            text-align: center;
            max-width: 90%;
            width: 90%;
        }

        #popupContent img {
            width: 100%;
            height: auto;
            border-radius: 20px;
        }

        #popupContent p {
            margin-top: 20px;
            font-size: 18px;
        }

        /* Diamond Points System */
        .diamond-container {
            position: relative;
            width: 80px;
            height: 80px;
            margin: 0 auto 20px auto;
        }

        .diamond {
            width: 100%;
            height: 100%;
            background: linear-gradient(145deg, #2193b0, #6dd5ed);
            box-shadow: 0 0 25px rgba(0, 255, 255, 0.7), inset 0 0 15px rgba(255, 255, 255, 0.5);
            transform: rotate(45deg);
            border-radius: 15px;
            overflow: hidden;
            position: relative;
        }

        .points {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) rotate(-45deg);
            font-family: 'Orbitron', sans-serif;
            font-size: 24px;
            font-weight: 800;
            color: white;
            text-shadow: 0 0 20px rgba(255, 255, 255, 1), 0 0 40px rgba(0, 255, 255, 0.8);
        }

        .lightning {
            position: absolute;
            width: 140%;
            height: 140%;
            top: -20%;
            left: -20%;
            background: radial-gradient(circle, rgba(255,255,255,0.5) 20%, rgba(0,0,0,0) 70%);
            opacity: 0.2;
            animation: flicker 1.5s infinite alternate;
            pointer-events: none;
        }

        .sparkles::before, .sparkles::after {
            content: '';
            position: absolute;
            width: 10px;
            height: 10px;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 50%;
            animation: sparkleMove 2s infinite ease-in-out;
        }

        .sparkles::after {
            width: 15px;
            height: 15px;
            background: rgba(255, 255, 0, 0.8);
            animation-delay: 1s;
        }

        .sparkles {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        @keyframes flicker {
            from { opacity: 0.2; }
            to { opacity: 0.7; }
        }

        @keyframes sparkleMove {
            0% { transform: translate(20px, 30px); opacity: 1; }
            50% { transform: translate(70px, 80px); opacity: 0.5; }
            100% { transform: translate(20px, 30px); opacity: 1; }
        }

        @keyframes glowPulse {
            0% { box-shadow: 0 0 30px rgba(255, 255, 255, 0.4); }
            50% { box-shadow: 0 0 60px rgba(255, 255, 255, 1); }
            100% { box-shadow: 0 0 30px rgba(255, 255, 255, 0.4); }
        }

        /* Hint and Backspace Buttons */
        .buttons-container {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
        }

        .hint-btn, .backspace-btn {
            position: relative;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s ease-in-out;
            overflow: hidden;
        }

        .hint-btn {
            background: linear-gradient(45deg, #00fff2, #0088ff);
            box-shadow: 0 0 20px rgba(0, 255, 242, 0.8), 0 0 40px rgba(0, 136, 255, 0.6);
            animation: glow 2s infinite alternate, pulse 3s infinite;
        }

        .backspace-btn {
            background: linear-gradient(45deg, #ffd700, #0000ff);
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.8), 0 0 40px rgba(0, 0, 255, 0.6);
            animation: glow 2s infinite alternate;
        }

        .hint-btn:hover, .backspace-btn:hover {
            transform: scale(1.1);
        }

        .hint-btn svg, .backspace-btn svg {
            width: 30px;
            height: 30px;
            fill: white;
        }

        .hint-box {
            position: absolute;
            top: 140px;
            width: 320px;
            padding: 20px;
            background: rgba(0, 0, 0, 0.9);
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(0, 255, 242, 0.9), 0 0 40px rgba(0, 136, 255, 0.8);
            color: white;
            text-align: center;
            font-size: 20px;
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 3px;
            backdrop-filter: blur(10px);
            opacity: 0;
            transform: scale(0.8);
            transition: opacity 0.3s ease-in-out, transform 0.3s ease-in-out;
            font-family: 'Orbitron', sans-serif;
            background-image: radial-gradient(circle, rgba(0, 255, 255, 0.2) 20%, transparent 60%);
            background-size: 200% 200%;
            animation: hexGlow 4s infinite;
        }

        .hint-box.show {
            opacity: 1;
            transform: scale(1);
        }

        @keyframes hexGlow {
            0% { background-position: 0% 0%; }
            50% { background-position: 100% 100%; }
            100% { background-position: 0% 0%; }
        }

        /* Level Up Design */
        .level-container {
            position: fixed;
            top: 20px;
            right: 20px;
            background: radial-gradient(circle, #1f4068, #162842);
            border-radius: 20px;
            padding: 10px;
            width: 120px;
            text-align: center;
            box-shadow: 0px 0px 20px #00ffff;
            transition: transform 0.2s ease-in-out;
        }

        .level-badge {
            font-size: 20px;
            font-weight: bold;
            background: linear-gradient(45deg, #00aaff, #00ffcc);
            color: white;
            padding: 10px;
            border-radius: 10px;
            display: inline-block;
            box-shadow: 0px 0px 15px #00ffff;
            transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
        }

        .level-container.glow {
            transform: scale(1.1);
            box-shadow: 0px 0px 30px #00ffcc;
        }

        .level-badge.pulse {
            transform: scale(1.1);
            box-shadow: 0px 0px 20px #00ffcc;
        }

        .particles {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            pointer-events: none;
        }

        .particle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: #00ffff;
            border-radius: 50%;
            opacity: 0;
            animation: floatUp 1.5s linear infinite;
        }

        @keyframes floatUp {
            0% {
                transform: translateY(0) scale(1);
                opacity: 1;
            }
            100% {
                transform: translateY(-50px) scale(0);
                opacity: 0;
            }
        }

        /* Timer System */
        .timer-container {
            position: fixed;
            top: 20px;
            left: 20px;
            padding: 10px 20px;
            font-size: 24px;
            font-weight: bold;
            color: white;
            background: radial-gradient(circle, #a1c4fd, #c2e9fb);
            border-radius: 20px;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.9), inset 0 0 8px rgba(0, 0, 0, 0.3);
            animation: borderGlow 2s infinite alternate;
            border: 6px solid rgba(255, 255, 255, 0.9);
        }

        @keyframes borderGlow {
            0% { box-shadow: 0 0 20px rgba(255, 255, 255, 0.9), inset 0 0 8px rgba(0, 0, 0, 0.3); }
            100% { box-shadow: 0 0 30px rgba(255, 255, 255, 1), inset 0 0 10px rgba(0, 0, 0, 0.4); }
        }

        /* Word Tiles System */
        .blank-tiles {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-top: 20px;
        }

        .blank-tile {
            width: 50px;
            height: 60px;
            background: linear-gradient(145deg, #fff8f0, #e3cbb5);
            border: 2px solid #b08968;
            font-size: 28px;
            font-weight: bold;
            color: #5c4033;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 10px;
            box-shadow: inset 0 4px 8px rgba(255, 255, 255, 0.5), 0 6px 12px rgba(0, 0, 0, 0.3);
            transition: all 0.3s ease;
        }

        .word-tiles {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
            margin: 20px auto;
            max-width: 600px;
        }

        .word-tile {
            width: 50px;
            height: 50px;
            font-size: 28px;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            position: relative;
            color: white;
            background: linear-gradient(145deg, #c08d60, #9b6c48);
            border-radius: 10px;
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4), inset 0 4px 6px rgba(255,255,255,0.2);
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }

        .word-tile:hover {
            transform: scale(1.1);
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.5), 0 0 12px rgba(255, 255, 255, 0.4);
        }

        .word-tile:active {
            transform: scale(0.95);
        }

        .fade-out {
            opacity: 0;
            transform: scale(0.8);
            transition: opacity 0.5s, transform 0.3s;
        }

        /* Base Button Styling */
        .btn {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 14px 30px;
            border-radius: 14px;
            font-size: 20px;
            font-weight: bold;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            box-shadow: 0px 0px 20px rgba(255, 255, 255, 0.5);
            border: 2px solid rgba(255, 255, 255, 0.2);
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
        }

        /* Hover Effects */
        .btn:hover {
            transform: scale(1.05);
            box-shadow: 0px 0px 25px rgba(255, 255, 255, 0.7);
        }

        .btn:active {
            transform: scale(0.95);
            box-shadow: 0px 0px 15px rgba(255, 255, 255, 0.4);
        }

        /* NEXT QUESTION BUTTON */
        .next-btn {
            background: linear-gradient(135deg, #00c6ff, #0072ff);
            color: white;
            box-shadow: 0px 0px 15px rgba(0, 114, 255, 0.9);
            position: relative;
        }

        .next-btn::after {
            content: '➜';
            font-size: 24px;
            margin-left: 12px;
            display: inline-block;
            animation: emojiBounce 1s infinite ease-in-out;
        }

        @keyframes emojiBounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }

        /* REVIEW UNANSWERED QUESTIONS BUTTON */
        .review-btn {
            background: linear-gradient(135deg, #ff512f, #dd2476);
            color: white;
            box-shadow: 0px 0px 15px rgba(255, 46, 0, 0.9);
            position: relative;
        }

        .review-btn::after {
            content: '⏳';
            font-size: 24px;
            margin-left: 12px;
            display: inline-block;
            animation: rotateEmoji 2s linear infinite;
        }

        @keyframes rotateEmoji {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* TOUCH EFFECT */
        .touch-effect {
            position: absolute;
            width: 80px;
            height: 80px;
            background: rgba(255, 255, 255, 0.4);
            border-radius: 50%;
            transform: scale(0);
            animation: touchExpand 0.6s ease-out forwards;
            pointer-events: none;
        }

        @keyframes touchExpand {
            0% { transform: scale(0); opacity: 0.7; }
            100% { transform: scale(2); opacity: 0; }
        }

        /* SPARKLE EFFECT */
        .btn:hover::before {
            content: '✨';
            position: absolute;
            font-size: 26px;
            top: -10px;
            right: -10px;
            animation: sparkle 1.2s infinite ease-in-out;
        }

        @keyframes sparkle {
            0% { opacity: 0; transform: scale(0.5) rotate(0deg); }
            50% { opacity: 1; transform: scale(1.1) rotate(180deg); }
            100% { opacity: 0; transform: scale(0.5) rotate(360deg); }
        }

        /* Enhanced Description Box */
        .description-box {
            display: none;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 450px;
            padding: 30px;
            border-radius: 20px;
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.05));
            backdrop-filter: blur(20px);
            box-shadow: 0 0 40px rgba(255, 255, 255, 0.3);
            text-align: center;
            font-size: 20px;
            font-weight: bold;
            color: white;
            z-index: 100;
            border: 2px solid rgba(255, 255, 255, 0.3);
            animation: fadeIn 0.5s ease-in-out;
        }

        @keyframes fadeIn {
            0% { opacity: 0; transform: translate(-50%, -50%) scale(0.9); }
            100% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
        }

        /* Diamond Background Effect */
        .description-box::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: radial-gradient(circle, rgba(255, 255, 255, 0.1) 10%, transparent 10.01%);
            background-size: 20px 20px;
            z-index: -1;
            animation: diamondSparkle 3s infinite linear;
        }

        @keyframes diamondSparkle {
            0% { background-position: 0 0; }
            100% { background-position: 40px 40px; }
        }

        /* Description Text Styling */
        #descriptionText {
            font-family: 'Arial', sans-serif;
            font-size: 22px;
            font-weight: bold;
            color: #ffffff;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            margin-bottom: 20px;
        }

        /* Buttons Inside Description Box */
        .desc-btn {
            margin-top: 15px;
            padding: 12px 24px;
            border-radius: 10px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
            border: 2px solid rgba(255, 255, 255, 0.3);
            background: rgba(255, 255, 255, 0.1);
            color: white;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
        }

        .desc-btn:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.05);
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.4);
        }

        .yes-btn {
            background: linear-gradient(135deg, #00ffcc, #00ccaa);
            color: black;
        }

        .no-btn {
            background: linear-gradient(135deg, #ff4444, #cc0000);
            color: white;
        }

        .ok-btn {
            background: linear-gradient(135deg, #ffaa00, #ff8800);
            color: black;
        }

        /* Unanswered Questions Box */
        .unanswered-box {
            display: none;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 450px;
            padding: 30px;
            border-radius: 20px;
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.05));
            backdrop-filter: blur(20px);
            box-shadow: 0 0 40px rgba(255, 255, 255, 0.3);
            text-align: center;
            color: white;
            z-index: 100;
            border: 2px solid rgba(255, 255, 255, 0.3);
            animation: fadeIn 0.5s ease-in-out;
        }

        .unanswered-item {
            margin-top: 10px;
            padding: 12px;
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.1);
            cursor: pointer;
            transition: 0.3s;
            border: 2px solid rgba(255, 255, 255, 0.2);
        }

        .unanswered-item:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.05);
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.3);
        }

        /* Game Rules Styles */
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@600&display=swap');

        .rules-container {
            width: 85%;
            max-width: 650px;
            background: rgba(15, 15, 15, 0.95);
            border-radius: 15px;
            padding: 25px;
            position: relative;
            box-shadow: 0 0 20px rgba(0, 255, 255, 0.7);
            overflow: hidden;
            border: 3px solid transparent;
            animation: glowBorder 3s infinite alternate, fadeIn 1s ease-in-out;
            transition: all 0.3s ease-in-out;
            display: none; /* Initially hidden */
        }

        @keyframes glowBorder {
            0% { border-color: rgba(0, 255, 255, 0.8); box-shadow: 0 0 15px cyan; }
            100% { border-color: rgba(255, 0, 255, 0.8); box-shadow: 0 0 20px magenta; }
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        h2 {
            text-align: center;
            font-size: 26px;
            letter-spacing: 2px;
            background: linear-gradient(90deg, cyan, magenta);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 10px rgba(0, 255, 255, 0.7);
            animation: shimmerText 2s infinite alternate;
        }

        @keyframes shimmerText {
            0% { text-shadow: 0 0 10px cyan; }
            100% { text-shadow: 0 0 20px magenta; }
        }

        .rules-text {
            color: #ffffff;
            font-size: 16px;
            line-height: 1.6;
            text-shadow: 0 0 5px rgba(255, 255, 255, 0.4);
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInUp 1s ease-in-out forwards, textGlow 3s infinite alternate;
        }

        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes textGlow {
            0% { text-shadow: 0 0 10px cyan; }
            100% { text-shadow: 0 0 20px magenta; }
        }

        .rules-container:hover {
            box-shadow: 0 0 30px rgba(0, 255, 255, 1);
            transform: scale(1.02);
        }

        .close-btn {
            position: absolute;
            top: 10px;
            right: 15px;
            background: none;
            border: none;
            font-size: 22px;
            color: #ff0099;
            cursor: pointer;
            transition: transform 0.3s ease-in-out;
        }

        .close-btn:hover {
            transform: scale(1.3);
            color: #ffffff;
        }

        .highlight {
            background: linear-gradient(90deg, cyan, magenta);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            font-weight: bold;
            animation: glowHighlight 1.5s infinite alternate;
        }

        @keyframes glowHighlight {
            0% { text-shadow: 0 0 10px cyan; }
            100% { text-shadow: 0 0 20px magenta; }
        }

        /* Auto-Fade Effect */
        .rules-container.idle {
            opacity: 0.5;
            transform: scale(0.95);
            transition: opacity 1s, transform 1s;
        }

        /* Background Particles */
        .particles {
            position: absolute;
            width: 100%;
            height: 100%;
            overflow: hidden;
            top: 0;
            left: 0;
            pointer-events: none;
        }

        .particle {
            position: absolute;
            width: 5px;
            height: 5px;
            background: cyan;
            border-radius: 50%;
            opacity: 0.7;
            animation: moveParticles 5s linear infinite;
        }

        @keyframes moveParticles {
            0% { transform: translateY(0); opacity: 1; }
            100% { transform: translateY(-100vh); opacity: 0; }
        }

        /* Shining Box Design */
        .content-box {
            position: relative;
            padding: 30px 20px;
            width: 320px;
            border-radius: 20px;
            background: rgba(30, 30, 30, 0.9);
            color: white;
            text-align: center;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.1);
            overflow: hidden;
            animation: floatUp 1s ease, glowPulse 3s infinite;
            display: none;
        }

        .content-box::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, #ff00cc, #3333ff, #00ffcc, #ffcc00, #ff00cc);
            background-size: 400%;
            animation: shineRotate 6s linear infinite;
            filter: blur(20px);
            z-index: -1;
        }

        .close-btn {
            position: absolute;
            top: 12px;
            left: 12px;
            background: rgba(255, 255, 255, 0.15);
            border: none;
            color: white;
            font-size: 18px;
            padding: 5px 10px;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
        }

        .close-btn:hover {
            background: rgba(255, 255, 255, 0.35);
        }

        h2 {
            font-size: 24px;
            margin-bottom: 10px;
            font-weight: 700;
            letter-spacing: 1px;
        }

        p {
            font-size: 16px;
            opacity: 0.9;
            line-height: 1.6;
        }

        @keyframes shineRotate {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes glowPulse {
            0%, 100% { box-shadow: 0 0 20px rgba(255, 255, 255, 0.1); }
            50% { box-shadow: 0 0 30px rgba(255, 255, 255, 0.3); }
        }

        @keyframes floatUp {
            from { transform: translateY(30px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        .hidden {
            display: none;
        }

        /* Blur Background */
        .blur-background {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            backdrop-filter: blur(10px);
            z-index: 999;
            display: none;
        }

        /* Popup with Countdown */
        .popup-countdown {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.8);
            padding: 20px;
            border-radius: 20px;
            text-align: center;
            z-index: 1000;
            display: none;
        }

        .popup-countdown img {
            width: 100%;
            height: auto;
            border-radius: 20px;
        }

        .popup-countdown p {
            margin-top: 20px;
            font-size: 18px;
            color: white;
        }

        .popup-countdown .countdown-text {
            font-size: 24px;
            font-weight: bold;
            color: #00ffee;
            text-shadow: 0 0 15px rgba(0, 255, 238, 0.8);
        }
    </style>
</head>
<body>

    <!-- Animated Background -->
    <div class="background"></div>

    <!-- Loading Screen -->
    <div class="loading-container">
        <div class="word-tiles">
            <div class="tile" style="animation-delay: 0s;">L</div>
            <div class="tile" style="animation-delay: 0.2s;">O</div>
            <div class="tile" style="animation-delay: 0.4s;">A</div>
            <div class="tile" style="animation-delay: 0.6s;">D</div>
            <div class="tile" style="animation-delay: 0.8s;">I</div>
            <div class="tile" style="animation-delay: 1s;">N</div>
            <div class="tile" style="animation-delay: 1.2s;">G</div>
        </div>
        <button class="start-btn" onclick="startCountdown()">START</button>
        <div class="countdown" id="countdown"></div>
    </div>

    <!-- Game Rules -->
    <div id="rulesBox" class="rules-container">
        <h2>Game Rules</h2>
        <div class="rules-text">
            <p><span class="highlight">1. Objective:</span> Guess the correct word based on four given pictures.</p>
            <p><span class="highlight">2. Gameplay:</span> Choose letters to form the correct word.</p>
            <p><span class="highlight">3. Timer:</span> 60 seconds per question.</p>
            <p><span class="highlight">4. Points:</span> Earn up to <span class="highlight">3 points</span> based on speed.</p>
            <p><span class="highlight">5. Level Up:</span> Reach <span class="highlight">10 points</span> to level up.</p>
            <p><span class="highlight">6. Hints:</span> Reveal clues about the answer.</p>
            <p><span class="highlight">7. Backspace:</span> Remove selected letters.</p>
            <p><span class="highlight">8. Skip:</span> Confirm before skipping.</p>
            <p><span class="highlight">9. Review:</span> Revisit unanswered questions.</p>
            <p><span class="highlight">10. Game Over:</span> Final score displayed.</p>
        </div>
        <div class="particles"></div>
    </div>

    <!-- Game Container -->
    <div id="gameContainer">
        <!-- Diamond Points System -->
        <div class="diamond-container">
            <div class="diamond" id="diamond">
                <div class="lightning"></div>
                <div class="sparkles"></div>
                <div class="points" id="points">0</div>
            </div>
        </div>

        <!-- Level Up Design -->
        <div class="level-container" id="levelBox">
            <div class="level-badge" id="levelText">Level 1</div>
            <div class="particles" id="particleContainer"></div>
        </div>

        <!-- Timer System -->
        <div class="timer-container">
            <span id="timer">1:00</span>
        </div>

        <div class="images" id="images"></div>

        <!-- Word Tiles Section -->
        <div class="blank-tiles" id="blankTiles"></div>
        <div class="word-tiles" id="wordTiles"></div>

        <!-- Hint and Backspace Buttons -->
        <div class="buttons-container">
            <div class="hint-btn" onclick="showHint()">
                <svg viewBox="0 0 24 24">
                    <path d="M12 2C6.5 2 2 6.5 2 12s4.5 10 10 10 10-4.5 10-10S17.5 2 12 2zm1 16h-2v-2h2v2zm1.3-6.9l-.9.9c-.7.7-1.4 1.5-1.4 3h-2v-.5c0-1.2.5-2.3 1.4-3.1l1.2-1.2c.5-.5.8-1.2.8-2 0-1.7-1.3-3-3-3s-3 1.3-3 3H6c0-2.8 2.2-5 5-5s5 2.2 5 5c0 1.3-.5 2.6-1.3 3.5z"/>
                </svg>
            </div>
            <div class="backspace-btn" onclick="backspace()">
                <svg viewBox="0 0 24 24">
                    <path d="M22 4H8L2 12l6 8h14V4zm-5.2 11.2l-1.4 1.4L12 12l3.4-3.6 1.4 1.4L14.8 12l2 2.2z"/>
                </svg>
            </div>
        </div>

        <!-- Next Question and Review Buttons -->
        <div class="buttons-container">
            <button class="btn next-btn" onclick="createTouchEffect(event); showSkipDescription()">Skip This Question</button>
            <button class="btn review-btn" onclick="createTouchEffect(event); reviewUnanswered()">Review Unanswered Questions</button>
        </div>

        <div id="result"></div>
    </div>

    <!-- Description Box -->
    <div id="descriptionBox" class="description-box">
        <p id="descriptionText"></p>
        <div id="descriptionButtons">
            <button id="yesBtn" class="desc-btn yes-btn" onclick="confirmSkip()">Yes</button>
            <button id="noBtn" class="desc-btn no-btn" onclick="hideDescription()">No</button>
        </div>
    </div>

    <!-- Unanswered Questions Box -->
    <div id="unansweredBox" class="unanswered-box">
        <p>You have unanswered questions:</p>
        <div id="unansweredList"></div>
        <button class="desc-btn no-btn" onclick="hideUnansweredBox()">Close</button>
    </div>

    <!-- Shining Box Design -->
    <div id="shiningBox" class="content-box">
        <button class="close-btn" onclick="closeBox()">✖</button>
        <h2 id="shiningTitle"></h2>
        <p id="shiningDescription"></p>
    </div>

    <!-- Blur Background -->
    <div class="blur-background" id="blurBackground"></div>

    <!-- Popup with Countdown -->
    <div class="popup-countdown" id="popupCountdown">
        <img id="popupImage" src="" alt="Popup Image">
        <p id="popupDescription"></p>
        <div class="countdown-text" id="popupCountdownText">10</div>
    </div>

    <!-- Background Music -->
    <audio id="backgroundMusic" loop>
        <source src="background-music.mp3" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>

    <!-- Button Click Sound -->
    <audio id="buttonClickSound">
        <source src="button-click.mp3" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>

    <script>
        const questions = [
            {
                answer: "MOTHERBOARD",
                images: ["https://pcoutlet.com/wp-content/uploads/gigabyte-b760-ds3h-ac-ddr4.jpg", "https://m.media-amazon.com/images/I/71BW5+VGikL._AC_SL1500_.jpg", "https://www.fs-pcba.com/wp-content/uploads/2024/09/Motherboard-Area.png", "https://easypc.com.ph/cdn/shop/files/Gigabyte_Z790_UD_AX_LGA1700_Socket_DDR5_Motherboard-a_2048x.png?v=1708481947"],
                description: "A motherboard is the main circuit board in a PC, connecting all essential components like the CPU, RAM, storage, and peripherals. It facilitates communication between these parts and provides power, ensuring the system functions as a cohesive unit. Key features include slots for expansion, ports for connectivity, and chipsets that manage data flow."
            },
            {
                answer: "CMOS",
                images: ["https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2022/05/cmos-battery-backup.jpg", "https://lennox-it.uk/wp-content/uploads/2015/05/lithium_bios_battery.jpg", "https://d2kbvjszk9d5ln.cloudfront.net/yshop/upload/other/cmos-battery-20241029094830765.webp", "https://kmpic.asus.com/images/2013/12/26/4f570b20-993b-467b-a51f-83ff5d071483.png"],
                description: "CMOS (Complementary Metal Oxide Semiconductor) is used in computer processors, memory chips, and motherboards. It helps regulate electricity flow and stores BIOS settings, such as the system time and date."
            },
            {
                answer: "RAM",
                images: ["https://example.com/img9.jpg", "https://example.com/img10.jpg", "https://example.com/img11.jpg", "https://example.com/img12.jpg"],
                description: "RAM (Random Access Memory) is your computer or laptop's short-term memory. It's where the data is stored that your computer processor needs to run your applications and open your files."
            },
            {
                answer: "HDD",
                images: ["https://example.com/img13.jpg", "https://example.com/img14.jpg", "https://example.com/img15.jpg", "https://example.com/img16.jpg"],
                description: "A computer hard disk drive (HDD) is a non-volatile data storage device. Non-volatile refers to storage devices that maintain stored data when turned off."
            },
            {
                answer: "FDD",
                images: ["https://example.com/img17.jpg", "https://example.com/img18.jpg", "https://example.com/img19.jpg", "https://example.com/img20.jpg"],
                description: "FDD stands for floppy disk drive, a device that reads and writes data to floppy disks. Floppy disks were a common way to store and transfer data between computers from the 1970s to the 1990s."
            },
            {
                answer: "NORTHBRIDGE",
                images: ["https://example.com/img21.jpg", "https://example.com/img22.jpg", "https://example.com/img23.jpg", "https://example.com/img24.jpg"],
                description: "The northbridge is a component of a computer's motherboard that manages communication between the processor and other components."
            },
            {
                answer: "SOUTHBRIDGE",
                images: ["https://example.com/img25.jpg", "https://example.com/img26.jpg", "https://example.com/img27.jpg", "https://example.com/img28.jpg"],
                description: "The southbridge is a component on a computer's motherboard that manages input/output (I/O) devices. It works with the northbridge to control data flow and optimize system performance."
            },
            {
                answer: "POWERSUPPLY",
                images: ["https://example.com/img29.jpg", "https://example.com/img30.jpg", "https://example.com/img31.jpg", "https://example.com/img32.jpg"],
                description: "A power supply is an electrical device that provides electric power to an electrical load, such as a laptop, computer, server, or other electronic devices."
            },
            {
                answer: "HDMI",
                images: ["https://example.com/img33.jpg", "https://example.com/img34.jpg", "https://example.com/img35.jpg", "https://example.com/img36.jpg"],
                description: "Connects devices to displays for high-quality audio/video."
            },
            {
                answer: "KEYBOARD",
                images: ["https://example.com/img37.jpg", "https://example.com/img38.jpg", "https://example.com/img39.jpg", "https://example.com/img40.jpg"],
                description: "Input device for typing and commands."
            },
            {
                answer: "MOUSE",
                images: ["https://example.com/img41.jpg", "https://example.com/img42.jpg", "https://example.com/img43.jpg", "https://example.com/img44.jpg"],
                description: "Pointing device for navigation."
            },
            {
                answer: "ROUTER",
                images: ["https://example.com/img45.jpg", "https://example.com/img46.jpg", "https://example.com/img47.jpg", "https://example.com/img48.jpg"],
                description: "Directs internet traffic between devices and networks."
            },
            {
                answer: "PATCHPANEL",
                images: ["https://example.com/img49.jpg", "https://example.com/img50.jpg", "https://example.com/img51.jpg", "https://example.com/img52.jpg"],
                description: "Organizes and connects network cables."
            },
            {
                answer: "SWITCH",
                images: ["https://example.com/img53.jpg", "https://example.com/img54.jpg", "https://example.com/img55.jpg", "https://example.com/img56.jpg"],
                description: "Connects devices within a network."
            },
            {
                answer: "PRINTER",
                images: ["https://example.com/img57.jpg", "https://example.com/img58.jpg", "https://example.com/img59.jpg", "https://example.com/img60.jpg"],
                description: "Output device for printing documents."
            },
            {
                answer: "RJ45",
                images: ["https://example.com/img61.jpg", "https://example.com/img62.jpg", "https://example.com/img63.jpg", "https://example.com/img64.jpg"],
                description: "Standard connector for Ethernet cables."
            },
            {
                answer: "UTP",
                images: ["https://example.com/img65.jpg", "https://example.com/img66.jpg", "https://example.com/img67.jpg", "https://example.com/img68.jpg"],
                description: "Common cable for network connections."
            },
            {
                answer: "LAN TESTER",
                images: ["https://example.com/img69.jpg", "https://example.com/img70.jpg", "https://example.com/img71.jpg", "https://example.com/img72.jpg"],
                description: "Checks network cable connections."
            },
            {
                answer: "CRIMPING TOOL",
                images: ["https://example.com/img73.jpg", "https://example.com/img74.jpg", "https://example.com/img75.jpg", "https://example.com/img76.jpg"],
                description: "Attaches connectors to cables."
            },
            {
                answer: "PATCH DOWN",
                images: ["https://example.com/img77.jpg", "https://example.com/img78.jpg", "https://example.com/img79.jpg", "https://example.com/img80.jpg"],
                description: "Connects cables to patch panels."
            },
            {
                answer: "SCREWDRIVER",
                images: ["https://example.com/img81.jpg", "https://example.com/img82.jpg", "https://example.com/img83.jpg", "https://example.com/img84.jpg"],
                description: "Tightens or loosens screws (Philips/Flat head)."
            },
            {
                answer: "WIRE STRIPPER",
                images: ["https://example.com/img85.jpg", "https://example.com/img86.jpg", "https://example.com/img87.jpg", "https://example.com/img88.jpg"],
                description: "Removes insulation from wires."
            },
            {
                answer: "ANTI STATIC WRIST STRAP",
                images: ["https://example.com/img89.jpg", "https://example.com/img90.jpg", "https://example.com/img91.jpg", "https://example.com/img92.jpg"],
                description: "Prevents static damage to electronics."
            },
            {
                answer: "FLASH DRIVE",
                images: ["https://example.com/img93.jpg", "https://example.com/img94.jpg", "https://example.com/img95.jpg", "https://example.com/img96.jpg"],
                description: "Portable storage for transferring files."
            }
        ];

        let currentQuestion = 0;
        let points = 0;
        let timerInterval;
        let seconds = 60;
        let unansweredQuestions = [];
        let reviewChances = 3;

        // Background Music
        const backgroundMusic = document.getElementById("backgroundMusic");
        backgroundMusic.volume = 0.5; // Set volume to 50%
        backgroundMusic.play();

        // Button Click Sound
        const buttonClickSound = document.getElementById("buttonClickSound");

        // Show Start Button after Tile Animation
        setTimeout(() => {
            document.querySelector('.start-btn').style.display = 'block';
        }, 2000);

        function startCountdown() {
            document.querySelector('.start-btn').style.display = 'none';
            document.querySelector('.word-tiles').style.display = 'none';

            // Show Game Rules for 15 seconds
            document.getElementById('rulesBox').style.display = 'block';
            setTimeout(() => {
                document.getElementById('rulesBox').style.display = 'none';

                // Start 5-second countdown
                let countdownElement = document.getElementById('countdown');
                countdownElement.style.display = 'block';
                let countdown = 5;
                countdownElement.innerText = countdown;

                let interval = setInterval(() => {
                    countdown--;
                    countdownElement.innerText = countdown;

                    if (countdown <= 0) {
                        clearInterval(interval);
                        countdownElement.style.display = 'none';
                        document.querySelector('.loading-container').style.display = 'none';
                        document.getElementById('gameContainer').style.display = 'block';
                        loadQuestion();
                        startTimer();
                    }
                }, 1000);
            }, 15000); // 15 seconds delay
        }

        function startTimer() {
            seconds = 60;
            updateTimer();
            timerInterval = setInterval(updateTimer, 1000);
        }

        function updateTimer() {
            let mins = Math.floor(seconds / 60);
            let secs = seconds % 60;
            let timerElement = document.getElementById("timer");

            timerElement.textContent = `${mins}:${secs < 10 ? "0" : ""}${secs}`;

            if (seconds > 0) {
                seconds--;
            } else {
                clearInterval(timerInterval);
                nextQuestion();
            }
        }

        function loadQuestion() {
            document.getElementById('images').innerHTML = '';
            document.getElementById('result').innerHTML = '';
            document.getElementById('blankTiles').innerHTML = '';
            document.getElementById('wordTiles').innerHTML = '';

            const question = questions[currentQuestion];
            question.images.forEach(src => {
                const img = document.createElement('img');
                img.src = src;
                document.getElementById('images').appendChild(img);
            });

            // Create blank tiles
            const blankTilesContainer = document.getElementById('blankTiles');
            for (let i = 0; i < question.answer.length; i++) {
                const blankTile = document.createElement('div');
                blankTile.classList.add('blank-tile');
                blankTile.textContent = '?'; // Add question mark for unanswered tiles
                blankTilesContainer.appendChild(blankTile);
            }

            // Create word tiles
            const wordTilesContainer = document.getElementById('wordTiles');
            const shuffledLetters = question.answer.split('').sort(() => Math.random() - 0.5);
            shuffledLetters.forEach(letter => {
                const wordTile = document.createElement('div');
                wordTile.classList.add('word-tile');
                wordTile.textContent = letter;
                wordTile.addEventListener('click', () => handleTileClick(letter, wordTile));
                wordTilesContainer.appendChild(wordTile);
            });
        }

        function handleTileClick(letter, wordTile) {
            const blankTiles = document.querySelectorAll('.blank-tile');
            for (let i = 0; i < blankTiles.length; i++) {
                if (blankTiles[i].textContent === '?') {
                    blankTiles[i].textContent = letter;
                    wordTile.remove(); // Remove the clicked tile from the word tiles
                    break;
                }
            }
            checkAnswer();
        }

        function backspace() {
            const blankTiles = document.querySelectorAll('.blank-tile');
            for (let i = blankTiles.length - 1; i >= 0; i--) {
                if (blankTiles[i].textContent !== '?') {
                    const letter = blankTiles[i].textContent;
                    blankTiles[i].textContent = '?'; // Reset to question mark
                    returnLetterToPool(letter);
                    break;
                }
            }
        }

        function returnLetterToPool(letter) {
            const wordTilesContainer = document.getElementById('wordTiles');
            const wordTile = document.createElement('div');
            wordTile.classList.add('word-tile');
            wordTile.textContent = letter;
            wordTile.addEventListener('click', () => handleTileClick(letter, wordTile));
            wordTilesContainer.appendChild(wordTile);
        }

        function checkAnswer() {
            const blankTiles = document.querySelectorAll('.blank-tile');
            let userAnswer = '';
            blankTiles.forEach(tile => userAnswer += tile.textContent);

            if (userAnswer === questions[currentQuestion].answer) {
                addPoints();
                showPopupWithCountdown();
            }
        }

        function showPopupWithCountdown() {
            const question = questions[currentQuestion];
            document.getElementById('blurBackground').style.display = 'block';
            document.getElementById('popupCountdown').style.display = 'block';
            document.getElementById('popupImage').src = question.images[0];
            document.getElementById('popupDescription').textContent = question.description;

            let countdown = 10;
            document.getElementById('popupCountdownText').textContent = countdown;

            const countdownInterval = setInterval(() => {
                countdown--;
                document.getElementById('popupCountdownText').textContent = countdown;

                if (countdown <= 0) {
                    clearInterval(countdownInterval);
                    document.getElementById('blurBackground').style.display = 'none';
                    document.getElementById('popupCountdown').style.display = 'none';
                    nextQuestion();
                }
            }, 1000);
        }

        function addPoints() {
            if (seconds > 40) {
                points += 3;
            } else if (seconds > 20) {
                points += 2;
            } else {
                points += 1;
            }
            document.getElementById('points').textContent = points;

            if (points % 10 === 0) {
                levelUp();
            }
        }

        function nextQuestion() {
            currentQuestion++;
            if (currentQuestion >= questions.length) {
                alert(`Congratulations! You completed the game with ${points} points.`);
                location.reload();
            } else {
                loadQuestion();
                startTimer();
            }
        }

        function levelUp() {
            let level = Math.floor(points / 10) + 1;
            document.getElementById("levelText").innerText = "Level " + level;

            let levelBox = document.getElementById("levelBox");
            let levelBadge = document.getElementById("levelText");

            levelBox.classList.add("glow");
            levelBadge.classList.add("pulse");

            setTimeout(() => {
                levelBox.classList.remove("glow");
                levelBadge.classList.remove("pulse");
            }, 600);

            createParticles();
        }

        function createParticles() {
            let particleContainer = document.getElementById("particleContainer");
            particleContainer.innerHTML = "";

            for (let i = 0; i < 15; i++) {
                let particle = document.createElement("div");
                particle.classList.add("particle");

                let randomX = Math.random() * 100;
                let randomY = Math.random() * 20 + 80;
                let delay = Math.random() * 0.5;

                particle.style.left = randomX + "%";
                particle.style.bottom = randomY + "px";
                particle.style.animationDelay = delay + "s";

                particleContainer.appendChild(particle);
            }
        }

        function showHint() {
            const question = questions[currentQuestion];
            const hintBox = document.createElement('div');
            hintBox.classList.add('hint-box');
            hintBox.textContent = `Hint: ${question.description}`;
            document.body.appendChild(hintBox);
            setTimeout(() => hintBox.classList.add('show'), 10);
            setTimeout(() => hintBox.remove(), 5000);
        }

        function showSkipDescription() {
            document.getElementById("descriptionBox").style.display = "block";
            document.getElementById("descriptionText").innerText = "Are you sure you want to skip this question?";
            document.getElementById("yesBtn").style.display = "inline-block";
            document.getElementById("noBtn").style.display = "inline-block";
        }

        function confirmSkip() {
            document.getElementById("descriptionText").innerText = "No Points Awarded!";
            document.getElementById("yesBtn").style.display = "none";
            document.getElementById("noBtn").style.display = "none";

            let okBtn = document.createElement("button");
            okBtn.classList.add("desc-btn", "ok-btn");
            okBtn.innerText = "OK";
            okBtn.onclick = function() {
                hideDescription();
                nextQuestion();
            };
            document.getElementById("descriptionButtons").appendChild(okBtn);
        }

        function hideDescription() {
            document.getElementById("descriptionBox").style.display = "none";
            document.getElementById("descriptionButtons").innerHTML = `
                <button id="yesBtn" class="desc-btn yes-btn" onclick="confirmSkip()">Yes</button>
                <button id="noBtn" class="desc-btn no-btn" onclick="hideDescription()">No</button>
            `;
        }

        function reviewUnanswered() {
            if (reviewChances <= 0) {
                document.getElementById("descriptionBox").style.display = "block";
                document.getElementById("descriptionText").innerText = "No more chances to review unanswered questions!";
                document.getElementById("yesBtn").style.display = "none";
                document.getElementById("noBtn").style.display = "none";

                let okBtn = document.createElement("button");
                okBtn.classList.add("desc-btn", "ok-btn");
                okBtn.innerText = "OK";
                okBtn.onclick = function() {
                    hideDescription();
                };
                document.getElementById("descriptionButtons").appendChild(okBtn);
                return;
            }

            unansweredQuestions = [];
            for (let i = 0; i < questions.length; i++) {
                if (!questions[i].answered) {
                    unansweredQuestions.push(`Question ${i + 1}`);
                }
            }

            if (unansweredQuestions.length > 0) {
                document.getElementById("unansweredList").innerHTML = "";
                unansweredQuestions.forEach((question, index) => {
                    let questionItem = document.createElement("div");
                    questionItem.classList.add("unanswered-item");
                    questionItem.innerText = question;
                    questionItem.onclick = function() {
                        showProceedCancel(question);
                    };
                    document.getElementById("unansweredList").appendChild(questionItem);
                });

                document.getElementById("unansweredBox").style.display = "block";
            } else {
                document.getElementById("descriptionBox").style.display = "block";
                document.getElementById("descriptionText").innerText = "No unanswered questions!";
                document.getElementById("yesBtn").style.display = "none";
                document.getElementById("noBtn").style.display = "none";

                let okBtn = document.createElement("button");
                okBtn.classList.add("desc-btn", "ok-btn");
                okBtn.innerText = "OK";
                okBtn.onclick = function() {
                    hideDescription();
                };
                document.getElementById("descriptionButtons").appendChild(okBtn);
            }
        }

        function showProceedCancel(question) {
            document.getElementById("descriptionBox").style.display = "block";
            document.getElementById("descriptionText").innerText = `Do you want to proceed with ${question}?`;
            document.getElementById("yesBtn").style.display = "none";
            document.getElementById("noBtn").style.display = "none";

            let proceedBtn = document.createElement("button");
            proceedBtn.classList.add("desc-btn", "yes-btn");
            proceedBtn.innerText = "Proceed";
            proceedBtn.onclick = function() {
                hideDescription();
                reviewChances--;
                alert(`Proceeding to ${question}`);
            };

            let cancelBtn = document.createElement("button");
            cancelBtn.classList.add("desc-btn", "no-btn");
            cancelBtn.innerText = "Cancel";
            cancelBtn.onclick = function() {
                hideDescription();
            };

            document.getElementById("descriptionButtons").innerHTML = "";
            document.getElementById("descriptionButtons").appendChild(proceedBtn);
            document.getElementById("descriptionButtons").appendChild(cancelBtn);
        }

        function hideUnansweredBox() {
            document.getElementById("unansweredBox").style.display = "none";
        }

        function createTouchEffect(event) {
            let touch = document.createElement("div");
            touch.classList.add("touch-effect");
            document.body.appendChild(touch);

            touch.style.left = event.clientX - 40 + "px";
            touch.style.top = event.clientY - 40 + "px";

            setTimeout(() => touch.remove(), 600);
        }

        function closeRules() {
            const rulesBox = document.getElementById("rulesBox");
            rulesBox.style.opacity = "0";
            setTimeout(() => { rulesBox.style.display = "none"; }, 500);
        }

        function createParticles() {
            const particleContainer = document.querySelector(".particles");
            for (let i = 0; i < 30; i++) {
                let particle = document.createElement("div");
                particle.classList.add("particle");
                particle.style.left = Math.random() * 100 + "vw";
                particle.style.animationDuration = (Math.random() * 3 + 2) + "s";
                particleContainer.appendChild(particle);
            }
        }

        createParticles();
    </script>
</body>
</html>
