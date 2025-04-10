<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Interactive Webpage</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Montserrat', sans-serif;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      display: flex;
      min-height: 100vh;
      background-color: #f5f5f5;
      overflow-x: hidden;
    }

    /* Menu Bar with Tap Roller */
    .menu-bar {
      position: fixed;
      left: 0;
      top: 0;
      width: 80px;
      height: 100vh;
      background-color: #2c3e50;
      z-index: 100;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding-top: 20px;
    }

    .tap-roller-container {
      position: relative;
      width: 40px;
      height: calc(100vh - 100px);
      margin-top: 20px;
      overflow: hidden;
    }

    .tap-roller {
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 40px;
      height: 40px;
      background-color: #3498db;
      border-radius: 50%;
      transition: height 0.3s ease;
    }

    .tap-handle {
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 40px;
      height: 40px;
      background-color: #2980b9;
      border-radius: 50%;
      z-index: 2;
    }

    /* Main Content */
    .main-content {
      margin-left: 80px;
      width: calc(100% - 80px);
      padding: 20px;
    }

    /* Section with Cards */
    .section {
      min-height: 100vh;
      display: flex;
      align-items: center;
      position: relative;
      padding: 40px 0;
      overflow: hidden;
    }

    .section-title {
      position: fixed;
      left: 100px;
      width: 300px;
      font-size: 3rem;
      font-weight: 700;
      color: #2c3e50;
      z-index: 10;
      transform: translateY(-50%);
      top: 50%;
    }

    .section-title span {
      display: inline-block;
      opacity: 0;
      transform: translateY(20px);
      transition: all 0.5s ease;
    }

    .cards-container {
      margin-left: 400px;
      display: flex;
      flex-direction: column;
      gap: 30px;
      width: calc(100% - 400px);
      position: relative;
      height: 100%;
    }

    .card {
      background-color: white;
      border-radius: 20px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
      overflow: hidden;
      width: 100%;
      max-width: 600px;
      position: absolute;
      top: 0;
      left: 0;
      transition: all 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      transform: translateX(100%) rotateY(90deg);
      opacity: 0;
      height: auto;
    }

    .card.active {
      transform: translateX(0) rotateY(0deg);
      opacity: 1;
      position: relative;
    }

    .card-image {
      width: 100%;
      height: 300px;
      object-fit: cover;
    }

    .card-content {
      padding: 30px;
    }

    .card-content h3 {
      font-size: 1.8rem;
      margin-bottom: 15px;
      color: #2c3e50;
    }

    .card-content p {
      font-size: 1rem;
      line-height: 1.6;
      color: #7f8c8d;
    }

    /* Profile Section */
    .profile-section {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-bottom: 40px;
    }

    .profile-img-container {
      background: linear-gradient(45deg, #d1d9eb, #c1c9db, #d1d9eb);
      box-shadow: 
        7px 7px 17px -1px rgba(0,0,0,0.5), 
        -7px -7px 17px -1px rgba(255,255,255,0.7),
        inset 3px 3px 10px rgba(0,0,0,0.1),
        inset -3px -3px 10px rgba(255,255,255,0.5);
      display: flex;
      overflow: hidden;
      height: 180px;
      width: 180px;
      margin: 0 auto 20px;
      border-radius: 40% 60% 65% 35% / 35% 55% 45% 65%;
      animation: bari 12s linear infinite, float 6s ease-in-out infinite;
      justify-content: center;
      align-items: center;
      padding: 8px;
      transition: all 0.5s ease;
    }

    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }

    .profile-img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      object-fit: cover;
      transition: all 0.5s ease;
      cursor: pointer;
      position: relative;
      z-index: 2;
      border: 3px solid rgba(255,255,255,0.3);
      box-shadow: 0 0 20px rgba(0,0,0,0.1);
    }

    @keyframes bari {
      0% {border-radius: 40% 60% 65% 35% / 35% 55% 45% 65%; transform: rotate(0deg);}
      25% {border-radius: 65% 35% 40% 60% / 55% 40% 60% 45%;}
      50% {border-radius: 35% 65% 50% 50% / 65% 55% 45% 35%;}
      75% {border-radius: 55% 45% 45% 55% / 60% 45% 55% 40%;}
      100% {border-radius: 40% 60% 65% 35% / 35% 55% 45% 65%; transform: rotate(360deg);}
    }

    .profile-img-container::before {
      content: '';
      position: absolute;
      top: -5px;
      left: -5px;
      right: -5px;
      bottom: -5px;
      background: linear-gradient(45deg, #0077b5, #00b5ad, #0077b5);
      z-index: 1;
      border-radius: 50%;
      opacity: 0;
      transition: opacity 0.5s ease;
      animation: rotateBorder 8s linear infinite;
    }

    @keyframes rotateBorder {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .profile-img-container:hover::before {
      opacity: 0.7;
    }

    .profile-img-container:hover {
      transform: scale(1.05);
      box-shadow: 
        10px 10px 20px -1px rgba(0,0,0,0.4), 
        -10px -10px 20px -1px rgba(255,255,255,0.8),
        inset 5px 5px 15px rgba(0,0,0,0.1),
        inset -5px -5px 15px rgba(255,255,255,0.5);
    }

    .profile-img:hover {
      transform: scale(1.03);
      box-shadow: 0 0 30px rgba(0,0,0,0.2);
      border-color: rgba(255,255,255,0.5);
    }

    /* Shake and Bounce Animation */
    @keyframes shakeAndBounce {
      0%, 100% {
        transform: translateX(0) translateY(0);
      }
      10%, 30%, 50%, 70%, 90% {
        transform: translateX(-10px) translateY(-10px);
      }
      20%, 40%, 60%, 80% {
        transform: translateX(10px) translateY(10px);
      }
    }

    .profile-img.shake-and-bounce {
      animation: shakeAndBounce 0.5s ease 3;
    }

    .transforming-text {
      font-size: 1.5rem;
      margin: 10px 0;
      color: #333;
      text-shadow: 7px 7px 10px -2px rgba(0,0,0,0.5);
      transition: all 0.5s ease;
      height: 2.5rem;
      overflow: hidden;
      position: relative;
      width: 100%;
      text-align: center;
    }

    .transforming-text span {
      position: absolute;
      width: 100%;
      text-align: center;
      left: 0;
      opacity: 0;
      animation: textChange 12s infinite;
      font-size: 1.5rem;
      line-height: 1.2;
      padding: 0 10px;
      box-sizing: border-box;
    }

    .transforming-text span:nth-child(1) {
      animation-delay: 0s;
    }
    .transforming-text span:nth-child(2) {
      animation-delay: 3s;
    }
    .transforming-text span:nth-child(3) {
      animation-delay: 6s;
    }
    .transforming-text span:nth-child(4) {
      animation-delay: 9s;
    }

    @keyframes textChange {
      0% { opacity: 0; transform: translateY(20px); }
      10% { opacity: 1; transform: translateY(0); }
      20% { opacity: 1; transform: translateY(0); }
      30% { opacity: 0; transform: translateY(-20px); }
      100% { opacity: 0; transform: translateY(-20px); }
    }

    .bio {
      font-size: 1.2rem;
      color: #777;
      text-shadow: inset 7px 7px 10px -2px rgba(0,0,0,0.5), inset -7px -7px 10px -2px rgba(255,255,255,0.7);
      transition: all 0.5s ease;
    }

    /* Social Media Icons */
    .social-media {
      margin: 30px 0;
    }

    .social-media h2 {
      font-size: 2rem;
      margin-bottom: 15px;
      color: #333;
      transition: all 0.5s ease;
    }

    .icons {
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    .icons a {
      color: #333;
      font-size: 1.5rem;
      transition: all 0.3s ease;
      text-decoration: none;
      box-shadow: 7px 7px 17px -2px rgba(0,0,0,0.5), -7px -7px 17px -2px rgba(255,255,255,0.7);
      border-radius: 50%;
      padding: 8px;
      background-color: #d1d9eb;
      width: 40px;
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .icons a:hover {
      transform: translateY(-5px);
      box-shadow: inset 1px 1px 8px -2px rgba(0,0,0,0.5), inset -2px -2px 8px -2px rgba(255,255,255,0.7);
    }

    .icons a:hover i {
      font-size: 1.8rem;
    }

    .fa-tiktok {
      color: #000;
    }
    .fa-facebook {
      color: #0077b5;
    }
    .fa-whatsapp {
      color: green;
    }
    .fa-phone {
      color: #25D366;
    }
    .fa-instagram {
      color: #e1306c;
    }
    .fa-envelope {
      color: #d44638;
    }

    /* Audio Grid Sections */
    .audio-grid-section {
      display: none;
      min-height: 100vh;
      padding: 80px 20px;
    }

    .audio-grid-section.active {
      display: block;
    }

    .audio-grid-title {
      font-size: 2.5rem;
      color: #2c3e50;
      margin-bottom: 40px;
      text-align: center;
    }

    .audio-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 30px;
      max-width: 1200px;
      margin: 0 auto;
    }

    .audio-item {
      background-color: white;
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
      transition: transform 0.3s ease;
    }

    .audio-item:hover {
      transform: translateY(-5px);
    }

    .audio-player {
      width: 100%;
      margin-bottom: 15px;
    }

    .audio-info h3 {
      font-size: 1.2rem;
      color: #2c3e50;
      margin-bottom: 10px;
    }

    .audio-info p {
      font-size: 0.9rem;
      color: #7f8c8d;
      margin-bottom: 5px;
    }

    .audio-date {
      font-size: 0.8rem;
      color: #95a5a6;
      font-style: italic;
    }

    /* Bottom Navigation */
    .bottom-nav {
      position: fixed;
      bottom: 0;
      left: 80px;
      right: 0;
      height: 70px;
      background-color: white;
      box-shadow: 0 -5px 20px rgba(0, 0, 0, 0.1);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 100;
    }

    .nav-button {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      width: 80px;
      height: 100%;
      color: #7f8c8d;
      text-decoration: none;
      transition: all 0.3s ease;
    }

    .nav-button i {
      font-size: 1.5rem;
      margin-bottom: 5px;
    }

    .nav-button span {
      font-size: 0.7rem;
      font-weight: 500;
    }

    .nav-button.active {
      color: #3498db;
    }

    .nav-button:hover {
      color: #3498db;
      transform: translateY(-5px);
    }

    /* Form Modal */
    .form-modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.7);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.3s ease;
    }

    .form-modal.active {
      opacity: 1;
      pointer-events: all;
    }

    .form-container {
      background-color: white;
      border-radius: 20px;
      padding: 30px;
      width: 90%;
      max-width: 500px;
      transform: translateY(20px);
      transition: transform 0.3s ease;
    }

    .form-modal.active .form-container {
      transform: translateY(0);
    }

    .form-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }

    .form-header h2 {
      font-size: 1.5rem;
      color: #2c3e50;
    }

    .close-button {
      background: none;
      border: none;
      font-size: 1.5rem;
      color: #7f8c8d;
      cursor: pointer;
    }

    .form-group {
      margin-bottom: 20px;
    }

    .form-group label {
      display: block;
      margin-bottom: 8px;
      font-weight: 500;
      color: #2c3e50;
    }

    .form-group input,
    .form-group textarea {
      width: 100%;
      padding: 12px 15px;
      border: 1px solid #ddd;
      border-radius: 10px;
      font-size: 1rem;
    }

    .form-group textarea {
      min-height: 100px;
      resize: vertical;
    }

    .file-input {
      display: none;
    }

    .file-label {
      display: block;
      padding: 12px;
      background-color: #f5f5f5;
      border: 1px dashed #ddd;
      border-radius: 10px;
      text-align: center;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .file-label:hover {
      background-color: #e8f4fc;
      border-color: #3498db;
    }

    .file-info {
      font-size: 0.8rem;
      color: #7f8c8d;
      margin-top: 5px;
      text-align: center;
    }

    .submit-button {
      width: 100%;
      padding: 12px;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 10px;
      font-size: 1rem;
      font-weight: 500;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    .submit-button:hover {
      background-color: #2980b9;
    }

    /* Next Section */
    .next-section {
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #ecf0f1;
    }

    .next-section h2 {
      font-size: 3rem;
      color: #2c3e50;
    }

    /* Scroll Indicator */
    .scroll-indicator {
      position: fixed;
      right: 20px;
      top: 50%;
      transform: translateY(-50%);
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
      z-index: 100;
    }

    .dot {
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background-color: #bdc3c7;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .dot.active {
      background-color: #3498db;
      transform: scale(1.3);
    }

    /* Arena of Grace Text */
    .top-loader-text {
      position: fixed;
      top: 20px;
      left: 100px;
      font-size: 1rem;
      font-weight: bold;
      color: #fff;
      text-transform: uppercase;
      z-index: 1001;
      background-color: #0077b5;
      padding: 10px 20px;
      border-radius: 0 20px 20px 0;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
      animation: slideIn 0.5s forwards;
    }

    @keyframes slideIn {
      from {
        opacity: 0;
        transform: translateX(-50px);
      }
      to {
        opacity: 1;
        transform: translateX(0);
      }
    }

    /* Responsive Design */
    @media (max-width: 992px) {
      .section-title {
        position: static;
        width: 100%;
        margin-bottom: 30px;
        transform: none;
        text-align: center;
      }

      .cards-container {
        margin-left: 0;
        width: 100%;
      }

      .card {
        max-width: 100%;
      }

      .audio-grid {
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      }
    }

    @media (max-width: 768px) {
      .menu-bar {
        width: 60px;
      }

      .main-content {
        margin-left: 60px;
        width: calc(100% - 60px);
      }

      .bottom-nav {
        left: 60px;
      }

      .section-title {
        font-size: 2rem;
      }

      .audio-grid-title {
        font-size: 2rem;
      }

      .top-loader-text {
        left: 80px;
        font-size: 0.8rem;
        padding: 8px 15px;
      }
    }

    @media (max-width: 576px) {
      .nav-button {
        width: 60px;
      }

      .nav-button span {
        display: none;
      }

      .card-image {
        height: 200px;
      }

      .audio-grid {
        grid-template-columns: 1fr;
      }

      .top-loader-text {
        left: 60px;
        font-size: 0.7rem;
        padding: 6px 12px;
      }
    }
  </style>
</head>
<body>
  <!-- Menu Bar with Tap Roller -->
  <div class="menu-bar">
    <div class="tap-roller-container">
      <div class="tap-roller" id="tap-roller"></div>
      <div class="tap-handle"></div>
    </div>
  </div>

  <!-- Arena of Grace Text -->
  <div class="top-loader-text">Arena Of Grace</div>

  <!-- Scroll Indicator -->
  <div class="scroll-indicator" id="scroll-indicator">
    <div class="dot active" data-section="card-section"></div>
    <div class="dot" data-section="next-section"></div>
  </div>

  <!-- Main Content -->
  <div class="main-content">
    <!-- Section with Cards -->
    <div class="section" id="card-section">
      <h1 class="section-title" id="section-title">
        <span>Divine</span>
        <span>Inspiration</span>
        <span>For</span>
        <span>Your</span>
        <span>Journey</span>
      </h1>
      
      <div class="profile-section">
        <div class="profile-img-container">
          <img src="profile.jpg" alt="Profile Picture" class="profile-img">
        </div>
        <h1 class="transforming-text">
          <span>I Am The Konkonsa Prophet</span>
          <span>Man Of God</span>
          <span>Servant Of The Most High</span>
          <span>Messenger Of Truth</span>
        </h1>
        <p class="bio">Prophet | Martin Osei | Mensah</p>
        
        <div class="social-media">
          <div class="icons">
            <a href="https://www.tiktok.com/@prophet.osei?is_from_webapp=1&sender_device=pc" target="_blank"><i class="fab fa-tiktok"></i></a>
            <a href="" target="_blank"><i class="fab fa-facebook"></i></a>
            <a href="https://wa.me/+32492255205" target="_blank"><i class="fab fa-whatsapp"></i></a>
            <a href="tel:+32492255205" target="_blank"><i class="fas fa-phone"></i></a>
            <a href="" target="_blank"><i class="fab fa-instagram"></i></a>
          </div>
        </div>
      </div>
      
      <div class="cards-container" id="cards-container">
        <div class="card active" id="card1">
          <img src="https://source.unsplash.com/random/600x400?faith" alt="Faith" class="card-image">
          <div class="card-content">
            <h3>Faith & Belief</h3>
            <p>Discover the power of unwavering faith and how it can transform your life. Learn from ancient wisdom and modern testimonies.</p>
          </div>
        </div>
        
        <div class="card" id="card2">
          <img src="https://source.unsplash.com/random/600x400?prayer" alt="Prayer" class="card-image">
          <div class="card-content">
            <h3>Prayer & Meditation</h3>
            <p>Explore the profound effects of prayer and meditation on your spiritual and physical well-being. Techniques for deeper connection.</p>
          </div>
        </div>
        
        <div class="card" id="card3">
          <img src="https://source.unsplash.com/random/600x400?community" alt="Community" class="card-image">
          <div class="card-content">
            <h3>Community & Fellowship</h3>
            <p>The importance of spiritual community in your growth journey. Find your tribe and grow together in faith and purpose.</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Sermons Section -->
    <div class="audio-grid-section" id="sermons-section">
      <h2 class="audio-grid-title">Sermons</h2>
      <div class="audio-grid">
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>The Power of Faith</h3>
            <p>Discover how faith can move mountains in your life and bring miracles.</p>
            <p class="audio-date">June 15, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Walking in Love</h3>
            <p>Learn how to walk in love and transform your relationships.</p>
            <p class="audio-date">June 8, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Overcoming Challenges</h3>
            <p>Biblical principles for overcoming life's toughest challenges.</p>
            <p class="audio-date">June 1, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-4.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>The Joy of Giving</h3>
            <p>Discover the blessings that come from a generous heart.</p>
            <p class="audio-date">May 25, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-5.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Finding Your Purpose</h3>
            <p>God's plan for your life and how to discover your divine purpose.</p>
            <p class="audio-date">May 18, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-6.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Spiritual Warfare</h3>
            <p>Understanding and engaging in spiritual battles effectively.</p>
            <p class="audio-date">May 11, 2023</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Testimonies Section -->
    <div class="audio-grid-section" id="testimonies-section">
      <h2 class="audio-grid-title">Testimonies</h2>
      <div class="audio-grid">
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-7.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Healing Miracle</h3>
            <p>How God healed me from a chronic illness after years of suffering.</p>
            <p class="audio-date">June 12, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-8.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Financial Breakthrough</h3>
            <p>From debt to abundance - God's provision in impossible situations.</p>
            <p class="audio-date">June 5, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-9.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Family Restoration</h3>
            <p>How prayer brought my family back together after years of separation.</p>
            <p class="audio-date">May 29, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-10.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Deliverance Story</h3>
            <p>Freedom from addiction and spiritual oppression through Christ.</p>
            <p class="audio-date">May 22, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-11.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Career Blessing</h3>
            <p>How God opened doors no man could shut in my professional life.</p>
            <p class="audio-date">May 15, 2023</p>
          </div>
        </div>
        
        <div class="audio-item">
          <audio controls class="audio-player">
            <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-12.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
          </audio>
          <div class="audio-info">
            <h3>Answered Prayer</h3>
            <p>After years of waiting, God answered in ways I never imagined.</p>
            <p class="audio-date">May 8, 2023</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Next Section -->
    <div class="next-section" id="next-section">
      <h2>Continue Your Spiritual Journey</h2>
    </div>
  </div>

  <!-- Bottom Navigation -->
  <div class="bottom-nav">
    <a href="#" class="nav-button active" id="home-button">
      <i class="fas fa-home"></i>
      <span>Home</span>
    </a>
    <a href="#" class="nav-button" id="sermons-button">
      <i class="fas fa-church"></i>
      <span>Sermons</span>
    </a>
    <a href="#" class="nav-button" id="testimonies-button">
      <i class="fas fa-hand-holding-heart"></i>
      <span>Testimonies</span>
    </a>
    <a href="#" class="nav-button" id="chat-button">
      <i class="fas fa-comments"></i>
      <span>Chat</span>
    </a>
    <a href="#" class="nav-button" id="others-button">
      <i class="fas fa-ellipsis-h"></i>
      <span>Others</span>
    </a>
  </div>

  <!-- Form Modal -->
  <div class="form-modal" id="form-modal">
    <div class="form-container">
      <div class="form-header">
        <h2>Contact Form</h2>
        <button class="close-button" id="close-button">&times;</button>
      </div>
      <form id="contact-form">
        <div class="form-group">
          <label for="name">Name</label>
          <input type="text" id="name" name="name" required>
        </div>
        <div class="form-group">
          <label for="contact">Contact</label>
          <input type="text" id="contact" name="contact" required>
        </div>
        <div class="form-group">
          <label for="message">Message</label>
          <textarea id="message" name="message" required></textarea>
        </div>
        <div class="form-group">
          <input type="file" id="file-input" class="file-input" name="attachment">
          <label for="file-input" class="file-label">Click to attach files</label>
          <div class="file-info">Attach screenshots or files of offerings if any</div>
        </div>
        <button type="submit" class="submit-button">Submit</button>
      </form>
    </div>
  </div>

  <script>
    document.addEventListener('DOMContentLoaded', function() {
      // Elements
      const tapRoller = document.getElementById('tap-roller');
      const sectionTitle = document.getElementById('section-title');
      const titleSpans = document.querySelectorAll('.section-title span');
      const cards = [document.getElementById('card1'), document.getElementById('card2'), document.getElementById('card3')];
      const cardSection = document.getElementById('card-section');
      const nextSection = document.getElementById('next-section');
      const formModal = document.getElementById('form-modal');
      const othersButton = document.getElementById('others-button');
      const closeButton = document.getElementById('close-button');
      const navButtons = document.querySelectorAll('.nav-button');
      const scrollIndicator = document.getElementById('scroll-indicator');
      const dots = document.querySelectorAll('.dot');
      const cardsContainer = document.getElementById('cards-container');
      const homeButton = document.getElementById('home-button');
      const sermonsButton = document.getElementById('sermons-button');
      const testimoniesButton = document.getElementById('testimonies-button');
      const sermonsSection = document.getElementById('sermons-section');
      const testimoniesSection = document.getElementById('testimonies-section');
      const allSections = [cardSection, sermonsSection, testimoniesSection, nextSection];
      const profileImg = document.querySelector('.profile-img');
      const backgroundMusic = new Audio('music.mp3');
      let isMusicPlaying = false;

      // Scroll Animation Variables
      let currentCard = 0;
      let titleAnimationDone = false;
      let isScrolling = false;
      let scrollTimeout;

      // Initialize cards
      function initCards() {
        cards.forEach((card, index) => {
          if (index !== 0) {
            card.style.display = 'none';
          } else {
            card.style.display = 'block';
            card.classList.add('active');
          }
        });
      }

      // Show section and hide others
      function showSection(section) {
        allSections.forEach(sec => {
          sec.style.display = 'none';
          sec.classList.remove('active');
        });
        
        section.style.display = 'block';
        section.classList.add('active');
        window.scrollTo(0, 0);
      }

      // Tap Roller Animation
      function updateTapRoller(scrollPercentage) {
        const rollerHeight = scrollPercentage * (window.innerHeight - 140);
        tapRoller.style.height = `${40 + rollerHeight}px`;
      }

      // Animate Title
      function animateTitle() {
        if (titleAnimationDone) return;
        
        titleSpans.forEach((span, index) => {
          setTimeout(() => {
            span.style.opacity = '1';
            span.style.transform = 'translateY(0)';
          }, index * 200);
        });
        
        titleAnimationDone = true;
      }

      // Show Card
      function showCard(index) {
        if (index < 0 || index >= cards.length) return;
        
        // Hide all cards
        cards.forEach(card => {
          card.classList.remove('active');
          setTimeout(() => {
            card.style.display = 'none';
          }, 300);
        });
        
        // Show selected card
        cards[index].style.display = 'block';
        setTimeout(() => {
          cards[index].classList.add('active');
        }, 50);
        
        currentCard = index;
      }

      // Update Scroll Indicator
      function updateScrollIndicator() {
        const cardSectionTop = cardSection.offsetTop;
        const cardSectionHeight = cardSection.offsetHeight;
        const scrollPosition = window.scrollY;
        
        if (scrollPosition < cardSectionTop + cardSectionHeight - 100) {
          dots[0].classList.add('active');
          dots[1].classList.remove('active');
        } else {
          dots[0].classList.remove('active');
          dots[1].classList.add('active');
        }
      }

      // Handle Wheel Event
      function handleWheel(e) {
        if (isScrolling) return;
        
        isScrolling = true;
        clearTimeout(scrollTimeout);
        scrollTimeout = setTimeout(() => {
          isScrolling = false;
        }, 800);
        
        if (e.deltaY > 0) {
          // Scroll down
          if (currentCard < cards.length - 1) {
            showCard(currentCard + 1);
          } else {
            // Scroll to next section
            nextSection.scrollIntoView({ behavior: 'smooth' });
          }
        } else {
          // Scroll up
          if (currentCard > 0) {
            showCard(currentCard - 1);
          }
        }
        
        e.preventDefault();
      }

      // Handle Dot Click
      function handleDotClick(e) {
        const section = e.target.getAttribute('data-section');
        document.getElementById(section).scrollIntoView({ behavior: 'smooth' });
      }

      // Form Modal Toggle
      othersButton.addEventListener('click', function(e) {
        e.preventDefault();
        formModal.classList.add('active');
      });

      closeButton.addEventListener('click', function() {
        formModal.classList.remove('active');
      });

      // Navigation Buttons
      homeButton.addEventListener('click', function(e) {
        e.preventDefault();
        navButtons.forEach(btn => btn.classList.remove('active'));
        this.classList.add('active');
        showSection(cardSection);
      });

      sermonsButton.addEventListener('click', function(e) {
        e.preventDefault();
        navButtons.forEach(btn => btn.classList.remove('active'));
        this.classList.add('active');
        showSection(sermonsSection);
      });

      testimoniesButton.addEventListener('click', function(e) {
        e.preventDefault();
        navButtons.forEach(btn => btn.classList.remove('active'));
        this.classList.add('active');
        showSection(testimoniesSection);
      });

      // Profile Image Click - Music Toggle
      profileImg.addEventListener('click', function() {
        if (isMusicPlaying) {
          backgroundMusic.pause();
          isMusicPlaying = false;
        } else {
          backgroundMusic.play().catch(e => console.log("Auto-play prevented:", e));
          isMusicPlaying = true;
        }
        
        // Add shake animation
        profileImg.classList.add('shake-and-bounce');
        setTimeout(() => {
          profileImg.classList.remove('shake-and-bounce');
        }, 1500);
      });

      // Form Submission
      document.getElementById('contact-form').addEventListener('submit', function(e) {
        e.preventDefault();
        alert('Form submitted successfully!');
        formModal.classList.remove('active');
        this.reset();
      });

      // Dot Click Events
      dots.forEach(dot => {
        dot.addEventListener('click', handleDotClick);
      });

      // Scroll Event Listener
      window.addEventListener('scroll', function() {
        const scrollPosition = window.scrollY;
        const cardSectionTop = cardSection.offsetTop;
        const cardSectionHeight = cardSection.offsetHeight;
        const windowHeight = window.innerHeight;
        
        // Calculate scroll percentage for tap roller
        const maxScroll = cardSectionHeight - windowHeight;
        const scrollPercentage = Math.min(Math.max((scrollPosition - cardSectionTop) / maxScroll, 0), 1);
        updateTapRoller(scrollPercentage);
        
        // Update scroll indicator
        updateScrollIndicator();
      });

      // Wheel Event Listener for Card Swiping
      cardsContainer.addEventListener('wheel', handleWheel, { passive: false });

      // Touch Events for Mobile Swiping
      let touchStartX = 0;
      let touchStartY = 0;
      
      cardsContainer.addEventListener('touchstart', function(e) {
        touchStartX = e.touches[0].clientX;
        touchStartY = e.touches[0].clientY;
      }, { passive: true });
      
      cardsContainer.addEventListener('touchend', function(e) {
        if (isScrolling) return;
        
        const touchEndX = e.changedTouches[0].clientX;
        const touchEndY = e.changedTouches[0].clientY;
        const diffX = touchStartX - touchEndX;
        const diffY = touchStartY - touchEndY;
        
        // Check if it's a horizontal swipe
        if (Math.abs(diffX) > Math.abs(diffY)) {
          if (diffX > 50) {
            // Swipe left - next card
            if (currentCard < cards.length - 1) {
              showCard(currentCard + 1);
            } else {
              nextSection.scrollIntoView({ behavior: 'smooth' });
            }
          } else if (diffX < -50) {
            // Swipe right - previous card
            if (currentCard > 0) {
              showCard(currentCard - 1);
            }
          }
        }
      }, { passive: true });

      // Initial Animations
      initCards();
      animateTitle();
      showCard(0);
      showSection(cardSection);
    });
  </script>
</body>
</html>
