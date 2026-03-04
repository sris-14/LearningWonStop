#Learning
This repository contains my learning materials and projects related to various programming languages and technologies. 
It serves as a personal collection of resources, code snippets, and projects that I have worked on to enhance my skills and knowledge.  



<!--
  ================================================================
  templates/index.html
  ================================================================
  This is a Jinja2 template — it's HTML that Flask fills with data.

  Jinja2 syntax you'll see throughout this file:
              → outputs a variable's value
     → closes the block

  Flask automatically looks for templates inside the /templates folder.
  ================================================================
-->
<!DOCTYPE html>
<!-- data-theme="dark" is read by CSS to apply the dark color palette.
     JavaScript will toggle this to "light" when the user clicks the moon button. -->
<html lang="en" data-theme="dark">
<head>
  <meta charset="UTF-8" />
  <!-- viewport meta tag makes the site responsive on mobile devices -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>

  <!-- Your Name injects the name from app.py's PERSONAL dict -->
  <title>Your Name — Portfolio</title>

  <!-- SEO meta tags — help search engines understand your page -->
  <meta name="description" content="Portfolio of Your Name — Full-Stack Engineer &amp; AI Builder">
  <meta name="author" content="Your Name">

  <!-- Google Fonts: Syne = display/heading font, Fira Code = monospace/code font -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Fira+Code:wght@300;400;500&display=swap" rel="stylesheet">

  <style>
    /* ============================================================
       CSS CUSTOM PROPERTIES (Variables)
       Defined on :root so every element can access them.
       :root is a CSS pseudo-class that targets the root element of your document. In HTML, this is the <html> element.
       Think of these like constants you set once and reuse everywhere.
       To change the whole color scheme, just edit values here.

       N.B. : It has higher specificity.
    ============================================================ */
    :root {
      /* --- Dark theme (default) --- */
      --bg:        #050b18;           /* Main page background — deep space */
      --bg2:       #070f20;           /* Slightly lighter bg, used for footer */
      --surface:   rgba(10,20,45,0.7);   /* Card / panel backgrounds */
      --surface2:  rgba(14,30,65,0.85);  /* More opaque panels */

      /* Brand colors — blue/cyan/green palette */
      --blue:      #0ea5e9;           /* Primary accent — sky blue */
      --blue2:     #38bdf8;           /* Lighter blue for hover states */
      --cyan:      #06b6d4;           /* Secondary accent — cyan */
      --green:     #10d9a0;           /* Tertiary accent — mint green */

      /* Typography colors */
      --text:      #e2eaf5;           /* Main body text */
      --text2:     #8ba3c7;           /* Muted / secondary text */
      --text3:     #4a6285;           /* Very muted text (timestamps, labels) */

      /* Border and glow effects */
      --border:    rgba(14,165,233,0.15);  /* Subtle borders */
      --border2:   rgba(14,165,233,0.3);   /* More visible borders on hover */
      --glow:      rgba(14,165,233,0.2);   /* Box shadow glow */
      --glow2:     rgba(14,165,233,0.5);   /* Stronger glow */

      --card-bg:   rgba(7,18,42,0.8);      /* Project card background */
      --nav-bg:    rgba(5,11,24,0.85);     /* Navigation bar background */
    }

    /* --- Light theme overrides ---
       When <html data-theme="light">, these values replace the :root ones above.
       Only the values that CHANGE need to be listed here. */
    [data-theme="light"] {
      --bg:        #f0f4ff;
      --bg2:       #e4ecff;
      --surface:   rgba(255,255,255,0.75);
      --surface2:  rgba(230,240,255,0.9);
      --text:      #0f1f3d;
      --text2:     #2d5086;
      --text3:     #6a8fc0;
      --border:    rgba(14,165,233,0.2);
      --border2:   rgba(14,165,233,0.4);
      --glow:      rgba(14,165,233,0.15);
      --card-bg:   rgba(255,255,255,0.85);
      --nav-bg:    rgba(240,244,255,0.9);
    }

    /* Universal box-model reset — makes sizing predictable */
    *, *::before, *::after {
      box-sizing: border-box;  /* padding/border included in width/height */
      margin: 0;
      padding: 0;
    }

    html { scroll-behavior: smooth; } /* Smooth scroll when clicking anchor links */

    body {
      font-family: 'Syne', sans-serif;
      background: var(--bg);
      color: var(--text);
      overflow-x: hidden;  /* Prevent horizontal scroll from animations */
      cursor: none;        /* Hide default cursor — we use a custom one */
    }

    /* ============================================================
       CUSTOM CURSOR
       Two elements: a small filled dot (.cursor) that follows
       instantly, and a larger ring (.cursor-ring) that follows
       with a slight delay (giving a "lag" feel).
       Both are positioned with position:fixed so they stay
       on screen regardless of scroll position.
    ============================================================ */
    .cursor {
      position: fixed;
      width: 10px; height: 10px;
      background: var(--blue);
      border-radius: 50%;          /* Makes it a circle */
      pointer-events: none;        /* Clicks pass through to elements beneath */
      z-index: 99999;              /* Always on top of everything */
      transform: translate(-50%, -50%); /* Center on the actual mouse position */
      transition: width 0.2s, height 0.2s, background 0.2s;
    }

    .cursor-ring {
      position: fixed;
      width: 38px; height: 38px;
      border: 1.5px solid var(--blue);
      border-radius: 50%;
      pointer-events: none;
      z-index: 99998;
      transform: translate(-50%, -50%);
      opacity: 0.55;
      /* The ring transitions smoothly — this makes it lag behind the dot */
      transition: width 0.3s ease, height 0.3s ease;
    }

    /* When hovering over cards, change cursor to green and grow it */
    body:has(.project-card:hover) .cursor       { width: 18px; height: 18px; background: var(--green); }
    body:has(.project-card:hover) .cursor-ring  { width: 58px; height: 58px; border-color: var(--green); }

    /* Grow cursor on interactive elements */
    body:has(button:hover, a:hover, input:hover, textarea:hover) .cursor-ring { width: 50px; height: 50px; }

    /* ============================================================
       SOLAR SYSTEM CANVAS
       A <canvas> element where JavaScript draws the animation.
       It's fixed so it stays in place while the page scrolls.
       z-index: 0 puts it behind all other content.
       pointer-events: none lets clicks pass through to the page.
    ============================================================ */
    #solarCanvas {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      z-index: 0;
      pointer-events: none;
    }

    /* ============================================================
       NAVIGATION BAR
       position:fixed keeps it at the top while scrolling.
       backdrop-filter: blur() makes the glass-morphism effect
       (frosted glass look behind the nav).
    ============================================================ */
    nav {
      position: fixed;
      top: 0; left: 0; right: 0;
      z-index: 1000;                  /* High z-index so it's always on top */
      background: var(--nav-bg);
      backdrop-filter: blur(20px);    /* Frosted glass blur effect */
      -webkit-backdrop-filter: blur(20px); /* Safari support */
      border-bottom: 1px solid var(--border);
      padding: 0 6%;
      height: 68px;
      display: flex;
      align-items: center;            /* Vertically center nav items */
      justify-content: space-between;
    }

    .nav-logo {
      font-size: 20px;
      font-weight: 800;
      letter-spacing: -0.5px;
      /* Gradient text trick: clip a gradient to the text shape */
      background: linear-gradient(135deg, var(--blue), var(--cyan), var(--green));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      text-decoration: none;
    }

    .nav-links {
      display: flex;
      gap: 32px;
      list-style: none;
    }

    .nav-links a {
      text-decoration: none;
      color: var(--text2);
      font-size: 12px;
      font-weight: 600;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      transition: color 0.2s;
      position: relative;   /* needed so the ::after pseudo-element is positioned relative to it */
    }

    /* Animated underline on hover — starts at width:0, expands to 100% */
    .nav-links a::after {
      content: '';
      position: absolute;
      bottom: -5px; left: 0;
      width: 0; height: 1px;
      background: var(--blue);
      transition: width 0.3s ease;
    }

    .nav-links a:hover         { color: var(--blue); }
    .nav-links a:hover::after  { width: 100%; }

    .nav-right { display: flex; align-items: center; gap: 14px; }

    /* Theme toggle button — circular icon button */
    .theme-toggle {
      background: var(--surface);
      border: 1px solid var(--border2);
      color: var(--text2);
      width: 40px; height: 40px;
      border-radius: 50%;
      cursor: none;
      display: flex; align-items: center; justify-content: center;
      font-size: 16px;
      transition: all 0.25s;
      backdrop-filter: blur(10px);
    }
    .theme-toggle:hover { border-color: var(--blue); box-shadow: 0 0 16px var(--glow); }

    /* Primary CTA button in nav */
    .hire-btn {
      background: linear-gradient(135deg, var(--blue), var(--cyan));
      color: #fff;
      border: none;
      padding: 9px 20px;
      border-radius: 100px;   /* Pill shape */
      font-family: 'Syne', sans-serif;
      font-size: 12px;
      font-weight: 700;
      cursor: none;
      transition: all 0.3s;
      text-decoration: none;
      letter-spacing: 0.04em;
    }
    .hire-btn:hover { transform: translateY(-2px); box-shadow: 0 8px 28px rgba(14,165,233,0.4); }

    /* ============================================================
       HERO SECTION
       min-height: 100vh makes it fill the entire viewport height.
       padding-top: 68px offsets the fixed nav so content isn't hidden.
    ============================================================ */
    #hero {
      position: relative;
      z-index: 10;
      min-height: 100vh;
      display: flex;
      align-items: center;
      padding: 68px 8% 0;
    }

    .hero-content { max-width: 740px; }

    /* Small badge above the heading — shows availability status */
    .hero-badge {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      background: var(--surface);
      border: 1px solid var(--border2);
      border-radius: 100px;
      padding: 7px 18px;
      font-size: 12px;
      font-family: 'Fira Code', monospace;
      color: var(--green);
      margin-bottom: 28px;
      backdrop-filter: blur(10px);
      /* animation: name duration easing fill-mode
         fadeInDown is defined in @keyframes below */
      animation: fadeInDown 0.8s ease forwards;
    }

    /* The pulsing green dot inside the badge */
    .badge-dot {
      width: 7px; height: 7px;
      background: var(--green);
      border-radius: 50%;
      animation: pulseDot 2s ease-in-out infinite;
    }

    @keyframes pulseDot {
      0%, 100% { opacity: 1; transform: scale(1); }
      50%       { opacity: 0.4; transform: scale(0.6); }
    }

    /* Hero heading — uses clamp() for fluid font size.
       clamp(min, preferred, max)
       → min: never smaller than 46px
       → preferred: 7vw (7% of viewport width, so it scales)
       → max: never larger than 88px */
    .hero-title {
      font-size: clamp(46px, 7vw, 88px);
      font-weight: 800;
      line-height: 1.0;
      letter-spacing: -2px;
      margin-bottom: 18px;
      opacity: 0;   /* Start invisible; animation fades it in */
      animation: fadeInUp 1s ease 0.2s forwards;
    }

    .hero-title .line1 { display: block; color: var(--text); }

    /* Animated gradient text for "Universes." */
    .hero-title .highlight {
      display: block;
      background: linear-gradient(135deg, var(--blue) 0%, var(--cyan) 45%, var(--green) 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      background-size: 200% 200%;
      animation: gradShift 4s ease infinite 1.2s; /* gradShift starts after 1.2s delay */
    }

    /* Moves the gradient background position back and forth = color shimmer */
    @keyframes gradShift {
      0%, 100% { background-position: 0% 50%; }
      50%       { background-position: 100% 50%; }
    }

    .hero-sub {
      font-size: clamp(15px, 2vw, 18px);
      color: var(--text2);
      line-height: 1.75;
      max-width: 540px;
      margin-bottom: 42px;
      opacity: 0;
      animation: fadeInUp 1s ease 0.45s forwards;
    }

    .hero-actions {
      display: flex;
      gap: 14px;
      flex-wrap: wrap;   /* Wraps to next line on small screens */
      opacity: 0;
      animation: fadeInUp 1s ease 0.65s forwards;
    }

    /* Primary (filled) CTA button */
    .btn-primary {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      background: linear-gradient(135deg, var(--blue), var(--cyan));
      color: #fff;
      text-decoration: none;
      padding: 13px 30px;
      border-radius: 100px;
      font-family: 'Syne', sans-serif;
      font-size: 15px;
      font-weight: 700;
      transition: all 0.3s;
      border: none;
      cursor: none;
    }
    .btn-primary:hover { transform: translateY(-3px); box-shadow: 0 12px 38px rgba(14,165,233,0.45); }

    /* Secondary (outlined) CTA button */
    .btn-outline {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      background: transparent;
      color: var(--text);
      text-decoration: none;
      padding: 12px 28px;
      border-radius: 100px;
      font-family: 'Syne', sans-serif;
      font-size: 15px;
      font-weight: 600;
      transition: all 0.3s;
      border: 1.5px solid var(--border2);
      cursor: none;
    }
    .btn-outline:hover { border-color: var(--blue); color: var(--blue); box-shadow: 0 0 20px var(--glow); }

    /* Stats row below the CTA buttons */
    .hero-stats {
      display: flex;
      gap: 42px;
      margin-top: 62px;
      opacity: 0;
      animation: fadeInUp 1s ease 0.9s forwards;
    }

    .stat-num {
      font-size: 30px;
      font-weight: 800;
      color: var(--blue);
      line-height: 1;
      margin-bottom: 4px;
    }
    .stat-label { font-size: 11px; color: var(--text3); letter-spacing: 0.07em; text-transform: uppercase; }

    /* Floating code decorations — positioned absolutely within #hero */
    .float-code {
      position: absolute;
      font-family: 'Fira Code', monospace;
      font-size: 11px;
      color: var(--blue);
      opacity: 0.14;
      pointer-events: none;
      animation: floatY 9s ease-in-out infinite;
      white-space: nowrap;
    }
    /* nth-child selectors give each float-code a different animation timing */
    .float-code:nth-child(2) { animation-delay: -2s; animation-duration: 11s; }
    .float-code:nth-child(3) { animation-delay: -4.5s; animation-duration: 13s; }
    .float-code:nth-child(4) { animation-delay: -7s; animation-duration: 10s; }

    @keyframes floatY {
      0%, 100% { transform: translateY(0) rotate(0deg); }
      33%       { transform: translateY(-18px) rotate(1deg); }
      66%       { transform: translateY(10px) rotate(-0.5deg); }
    }

    /* Blinking cursor in the typing animation */
    .typed-cursor {
      color: var(--green);
      animation: blink 0.75s step-end infinite;
      font-family: 'Fira Code', monospace;
    }
    @keyframes blink { 50% { opacity: 0; } }

    /* ============================================================
       SHARED SECTION STYLES
       All <section> elements get the same base padding and z-index.
       z-index: 10 ensures they're above the canvas (z-index: 0).
    ============================================================ */
    section {
      position: relative;
      z-index: 10;
      padding: 100px 8%;
    }

    /* Small monospace label above section titles */
    .section-label {
      font-family: 'Fira Code', monospace;
      font-size: 12px;
      color: var(--green);
      letter-spacing: 0.2em;
      text-transform: uppercase;
      margin-bottom: 10px;
    }

    /* Main section heading */
    .section-title {
      font-size: clamp(28px, 4vw, 48px);
      font-weight: 800;
      line-height: 1.1;
      letter-spacing: -1px;
      margin-bottom: 14px;
    }

    /* Blue gradient text used as accent within section titles */
    .section-title .accent {
      background: linear-gradient(90deg, var(--blue), var(--cyan));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .section-desc {
      font-size: 16px;
      color: var(--text2);
      max-width: 520px;
      line-height: 1.7;
      margin-bottom: 56px;
    }

    /* Horizontal divider line between sections */
    .section-divider {
      width: 48px; height: 3px;
      background: linear-gradient(90deg, var(--blue), var(--cyan));
      border-radius: 2px;
      margin-bottom: 40px;
    }

    /* ============================================================
       ABOUT SECTION
    ============================================================ */
    #about .about-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 64px;
      align-items: center;
    }

    .about-text p {
      font-size: 15.5px;
      color: var(--text2);
      line-height: 1.85;
      margin-bottom: 16px;
    }
    .about-text strong { color: var(--blue); font-weight: 700; }

    /* Info chips below the about paragraphs */
    .about-chips {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 24px;
    }

    .chip {
      display: flex;
      align-items: center;
      gap: 7px;
      background: var(--surface);
      border: 1px solid var(--border);
      padding: 7px 16px;
      border-radius: 100px;
      font-size: 13px;
      color: var(--text2);
      transition: all 0.2s;
    }
    .chip:hover { border-color: var(--border2); color: var(--blue); }
    .chip span  { font-size: 15px; }  /* emoji in the chip */

    /* Skills column */
    .skills-grid { display: flex; flex-direction: column; gap: 16px; }

    .skill-header {
      display: flex;
      justify-content: space-between;
      font-size: 13px;
      font-weight: 600;
      margin-bottom: 7px;
      color: var(--text);
    }
    /* The percentage shown on the right */
    .skill-pct { color: var(--blue); font-family: 'Fira Code', monospace; font-size: 12px; }

    .skill-bar {
      height: 4px;
      background: var(--surface);
      border-radius: 100px;
      overflow: hidden;
      position: relative;
    }

    .skill-fill {
      height: 100%;
      /* Gradient left-to-right on the bar */
      background: linear-gradient(90deg, var(--blue), var(--cyan));
      border-radius: 100px;
      width: 0;   /* Starts at 0; JavaScript animates it to the real value */
      transition: width 1.4s cubic-bezier(0.4, 0, 0.2, 1);  /* Ease-out curve */
      position: relative;
    }

    /* Glowing dot at the end of each skill bar */
    .skill-fill::after {
      content: '';
      position: absolute;
      right: 0; top: 50%;
      transform: translateY(-50%);
      width: 8px; height: 8px;
      background: var(--cyan);
      border-radius: 50%;
      box-shadow: 0 0 6px var(--cyan);
    }

    /* ============================================================
       PROJECT CARDS
       The most interactive part of the page.
       Each card has:
         - 3D tilt via JavaScript (transform: rotateX/rotateY)
         - Shine sweep on hover (::before pseudo-element)
         - Glow that follows the mouse (::after with CSS custom props)
    ============================================================ */
    #projects .projects-grid {
      display: grid;
      /* auto-fill = as many columns as fit; minmax(340px, 1fr) = min 340px, max 1fr */
      grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
      gap: 26px;
    }

    .project-card {
      background: var(--card-bg);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 30px;
      cursor: none;
      position: relative;
      overflow: hidden;       /* Clips the shine sweep and glow to the card's rounded corners */
      transition: border-color 0.3s, box-shadow 0.3s;

      /* transform-style: preserve-3d allows child elements to participate in 3D space */
      transform-style: preserve-3d;
      will-change: transform;  /* Hint to browser: this element will transform (GPU optimize) */

      /* Glass morphism background blur */
      backdrop-filter: blur(14px);
      -webkit-backdrop-filter: blur(14px);
    }

    /* --- SHINE SWEEP (::before pseudo-element) ---
       A white-gradient stripe that starts off-screen left,
       and on hover sweeps to the right like a "shine" passing over the card.
       It's the ::before content of each card. */
    .project-card::before {
      content: '';
      position: absolute;
      top: 0; left: -120%;  /* Start fully off-screen to the left */
      width: 55%;
      height: 100%;
      /* A diagonal white gradient — the "shine" beam */
      background: linear-gradient(
        115deg,
        transparent 0%,
        rgba(255,255,255,0.04) 25%,
        rgba(255,255,255,0.10) 50%,
        rgba(255,255,255,0.04) 75%,
        transparent 100%
      );
      transition: left 0.55s ease;
      pointer-events: none;
      z-index: 2;
    }

    /* On hover, move the shine from left:-120% to left:150% = sweeps across */
    .project-card:hover::before { left: 150%; }

    /* --- MOUSE-TRACKING GLOW (::after pseudo-element) ---
       A radial gradient centered at --mx, --my (set by JavaScript).
       JavaScript updates these CSS custom properties as the mouse moves,
       creating a glow that follows the cursor around the card. */
    .project-card::after {
      content: '';
      position: absolute;
      inset: 0;                /* Fills the entire card */
      border-radius: 20px;
      background: radial-gradient(
        circle at var(--mx, 50%) var(--my, 50%),
        var(--glow2) 0%,
        transparent 65%
      );
      opacity: 0;
      transition: opacity 0.4s;
      pointer-events: none;
      z-index: 0;
    }

    .project-card:hover              { border-color: var(--border2); }
    .project-card:hover::after       { opacity: 0.45; }

    /* Card header: icon on left, status badge on right */
    .card-top {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      margin-bottom: 20px;
      position: relative; z-index: 1;  /* Above the glow layer (z-index:0) */
    }

    /* Emoji icon in a rounded square */
    .card-icon {
      font-size: 34px;
      width: 62px; height: 62px;
      background: var(--surface);
      border-radius: 14px;
      display: flex;
      align-items: center;
      justify-content: center;
      border: 1px solid var(--border);
      transition: transform 0.3s, box-shadow 0.3s;
    }
    /* Icon "pops" forward on card hover — translateZ pushes it toward viewer in 3D */
    .project-card:hover .card-icon {
      transform: scale(1.08) translateZ(8px);
      box-shadow: 0 0 22px var(--glow);
    }

    /* Status badge — color differs by status type */
    .card-status {
      font-family: 'Fira Code', monospace;
      font-size: 10px;
      padding: 5px 11px;
      border-radius: 100px;
      border: 1px solid currentColor;  /* border uses same color as text */
      letter-spacing: 0.06em;
    }
    /* Color-coded status styles */
    .status-live { color: var(--green); background: rgba(16,217,160,0.08); }
    .status-beta { color: var(--blue2); background: rgba(56,189,248,0.08); }
    .status-open { color: var(--cyan);  background: rgba(6,182,212,0.08); }

    .card-category {
      font-family: 'Fira Code', monospace;
      font-size: 11px;
      color: var(--blue);
      letter-spacing: 0.1em;
      text-transform: uppercase;
      margin-bottom: 7px;
      position: relative; z-index: 1;
    }

    .card-title {
      font-size: 21px;
      font-weight: 800;
      letter-spacing: -0.5px;
      margin-bottom: 10px;
      position: relative; z-index: 1;
      transition: color 0.2s;
    }
    .project-card:hover .card-title { color: var(--blue2); }

    .card-desc {
      font-size: 14px;
      color: var(--text2);
      line-height: 1.7;
      margin-bottom: 20px;
      position: relative; z-index: 1;
    }

    /* Tech stack tags */
    .card-tech {
      display: flex;
      flex-wrap: wrap;
      gap: 7px;
      position: relative; z-index: 1;
    }

    .tech-tag {
      font-family: 'Fira Code', monospace;
      font-size: 10px;
      padding: 4px 10px;
      background: var(--surface);
      border-radius: 100px;
      color: var(--text3);
      border: 1px solid var(--border);
      transition: all 0.2s;
    }
    .project-card:hover .tech-tag { border-color: var(--border2); color: var(--blue2); }

    .card-footer {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 20px;
      padding-top: 16px;
      border-top: 1px solid var(--border);
      position: relative; z-index: 1;
    }

    .card-year {
      font-family: 'Fira Code', monospace;
      font-size: 11px;
      color: var(--text3);
    }

    /* Arrow button in card footer — rotates 45° on hover */
    .card-arrow {
      width: 34px; height: 34px;
      border-radius: 50%;
      border: 1px solid var(--border2);
      display: flex; align-items: center; justify-content: center;
      color: var(--blue);
      font-size: 14px;
      text-decoration: none;
      transition: all 0.3s;
    }
    .project-card:hover .card-arrow {
      background: var(--blue);
      color: white;
      transform: rotate(45deg) scale(1.1);  /* Diagonal = ↗ arrow direction */
      box-shadow: 0 0 14px var(--glow2);
    }

    /* ============================================================
       CONTACT SECTION
       Contains a working form that submits via JavaScript fetch()
       to the Flask /contact endpoint without refreshing the page.
    ============================================================ */
    #contact { padding-bottom: 120px; }

    /* Two-column layout: left = form, right = info */
    .contact-grid {
      display: grid;
      grid-template-columns: 1.1fr 0.9fr;
      gap: 56px;
      align-items: start;
    }

    /* Glass panel for the form */
    .contact-form-wrap {
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: 24px;
      padding: 44px;
      backdrop-filter: blur(16px);
    }

    .form-title {
      font-size: 26px;
      font-weight: 800;
      letter-spacing: -0.5px;
      margin-bottom: 6px;
    }

    .form-subtitle {
      font-size: 14px;
      color: var(--text2);
      margin-bottom: 30px;
      line-height: 1.6;
    }

    /* Form field groups */
    .form-group {
      display: flex;
      flex-direction: column;
      gap: 7px;
      margin-bottom: 18px;
    }

    .form-label {
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--text3);
      font-family: 'Fira Code', monospace;
    }

    /* All text inputs and the textarea share this style */
    .form-input {
      width: 100%;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 13px 16px;
      font-family: 'Syne', sans-serif;
      font-size: 14px;
      color: var(--text);
      outline: none;
      transition: border-color 0.2s, box-shadow 0.2s;
      resize: vertical;   /* Only allow vertical resize for textarea */
    }

    .form-input::placeholder { color: var(--text3); }

    /* Blue glow on focus */
    .form-input:focus {
      border-color: var(--border2);
      box-shadow: 0 0 0 3px var(--glow);
    }

    /* Two inputs side by side (name + email) */
    .form-row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 14px;
    }

    /* The submit button — full width, gradient, with hover lift */
    .form-submit {
      width: 100%;
      padding: 15px;
      background: linear-gradient(135deg, var(--blue), var(--cyan));
      color: white;
      border: none;
      border-radius: 12px;
      font-family: 'Syne', sans-serif;
      font-size: 15px;
      font-weight: 700;
      cursor: none;
      transition: all 0.3s;
      margin-top: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
    }
    .form-submit:hover { transform: translateY(-2px); box-shadow: 0 12px 36px rgba(14,165,233,0.4); }

    /* Loading spinner — visible when form is being submitted */
    .spinner {
      width: 16px; height: 16px;
      border: 2px solid rgba(255,255,255,0.3);
      border-top-color: white;
      border-radius: 50%;
      animation: spin 0.6s linear infinite;
      display: none;
    }
    @keyframes spin { to { transform: rotate(360deg); } }

    /* Toast notification — pops up at the bottom of screen after submission */
    .form-toast {
      display: none;
      margin-top: 14px;
      padding: 13px 18px;
      border-radius: 10px;
      font-size: 13px;
      font-weight: 600;
      text-align: center;
    }
    .form-toast.success {
      background: rgba(16,217,160,0.12);
      border: 1px solid rgba(16,217,160,0.35);
      color: var(--green);
    }
    .form-toast.error {
      background: rgba(239,68,68,0.1);
      border: 1px solid rgba(239,68,68,0.3);
      color: #f87171;
    }

    /* Right column — contact info */
    .contact-info { padding-top: 8px; }

    .contact-info-title {
      font-size: 32px;
      font-weight: 800;
      letter-spacing: -1px;
      line-height: 1.15;
      margin-bottom: 16px;
    }

    .contact-info-desc {
      font-size: 15px;
      color: var(--text2);
      line-height: 1.75;
      margin-bottom: 36px;
    }

    /* Individual contact detail rows (email, location, availability) */
    .contact-detail {
      display: flex;
      align-items: flex-start;
      gap: 16px;
      margin-bottom: 22px;
    }

    .contact-detail-icon {
      width: 44px; height: 44px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      display: flex; align-items: center; justify-content: center;
      font-size: 18px;
      flex-shrink: 0;  /* Don't shrink the icon when text is long */
    }

    .contact-detail-label {
      font-size: 11px;
      color: var(--text3);
      text-transform: uppercase;
      letter-spacing: 0.1em;
      margin-bottom: 3px;
      font-family: 'Fira Code', monospace;
    }

    .contact-detail-value {
      font-size: 14px;
      color: var(--text);
      font-weight: 600;
    }

    .contact-detail-value a {
      color: var(--blue);
      text-decoration: none;
      transition: opacity 0.2s;
    }
    .contact-detail-value a:hover { opacity: 0.75; }

    /* Social links row */
    .contact-socials {
      display: flex;
      gap: 12px;
      margin-top: 30px;
    }

    .social-link {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 9px 18px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      text-decoration: none;
      color: var(--text2);
      font-size: 13px;
      font-weight: 600;
      transition: all 0.2s;
    }
    .social-link:hover { border-color: var(--blue); color: var(--blue); box-shadow: 0 0 14px var(--glow); }

    /* ============================================================
       FOOTER
    ============================================================ */
    footer {
      position: relative;
      z-index: 10;
      background: var(--bg2);
      border-top: 1px solid var(--border);
      padding: 28px 8%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 14px;
    }

    .footer-copy {
      font-size: 12px;
      color: var(--text3);
      font-family: 'Fira Code', monospace;
    }
    .footer-copy span { color: var(--blue); }

    /* ============================================================
       SCROLL REVEAL ANIMATION
       Elements start invisible and shifted down (opacity:0, translateY:40px).
       JavaScript's IntersectionObserver adds .visible when they scroll into view,
       which triggers the CSS transition to make them appear.
    ============================================================ */
    .reveal {
      opacity: 0;
      transform: translateY(36px);
      transition: opacity 0.7s ease, transform 0.7s ease;
    }
    .reveal.visible { opacity: 1; transform: translateY(0); }

    /* Stagger delay — children reveal one after another */
    .reveal-stagger .reveal:nth-child(1) { transition-delay: 0.05s; }
    .reveal-stagger .reveal:nth-child(2) { transition-delay: 0.15s; }
    .reveal-stagger .reveal:nth-child(3) { transition-delay: 0.25s; }
    .reveal-stagger .reveal:nth-child(4) { transition-delay: 0.35s; }
    .reveal-stagger .reveal:nth-child(5) { transition-delay: 0.45s; }
    .reveal-stagger .reveal:nth-child(6) { transition-delay: 0.55s; }

    /* ============================================================
       REUSABLE KEYFRAME ANIMATIONS
    ============================================================ */
    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(30px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    @keyframes fadeInDown {
      from { opacity: 0; transform: translateY(-20px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* ============================================================
       RESPONSIVE — MOBILE BREAKPOINTS
       @media (max-width: Xpx) rules override desktop styles
       when the viewport is smaller than X pixels.
    ============================================================ */
    @media (max-width: 900px) {
      .contact-grid { grid-template-columns: 1fr; gap: 40px; }
    }

    @media (max-width: 768px) {
      nav { padding: 0 5%; }
      .nav-links { display: none; }   /* Hide nav links on mobile (add a hamburger menu if you want) */
      #hero { padding: 68px 5% 0; }
      section { padding: 70px 5%; }
      #about .about-grid { grid-template-columns: 1fr; gap: 40px; }
      #projects .projects-grid { grid-template-columns: 1fr; }
      .contact-form-wrap { padding: 28px 22px; }
      .form-row { grid-template-columns: 1fr; }  /* Stack name/email on mobile */
      .hero-stats { gap: 22px; }
      footer { flex-direction: column; text-align: center; }
      .float-code { display: none; }  /* Hide decorations on small screens */
    }
  </style>
</head>
<body>

  <!-- =====================================================
       CUSTOM CURSOR ELEMENTS
       Positioned by JavaScript on every mousemove event.
  ===================================================== -->
  <div class="cursor" id="cursor"></div>
  <div class="cursor-ring" id="cursorRing"></div>

  <!-- =====================================================
       SOLAR SYSTEM CANVAS
       JavaScript draws on this element in the <script> section.
  ===================================================== -->
  <canvas id="solarCanvas"></canvas>

  <!-- =====================================================
       NAVIGATION
  ===================================================== -->
  <nav>
    <!-- Logo — links back to top of page -->
    <a href="#hero" class="nav-logo">⬡ YOUR</a>

    <!-- Navigation links — anchor tags jump to section IDs -->
    <ul class="nav-links">
      <li><a href="#hero">Home</a></li>
      <li><a href="#about">About</a></li>
      <li><a href="#projects">Projects</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>

    <div class="nav-right">
      <!-- Theme toggle: clicking this calls toggleTheme() in JavaScript -->
      <button class="theme-toggle" id="themeToggle" aria-label="Toggle theme">🌙</button>
      <a href="#contact" class="hire-btn">Hire Me ↗</a>
    </div>
  </nav>

  <!-- =====================================================
       HERO SECTION
  ===================================================== -->
  <section id="hero">
    <div class="hero-content">

      <!-- Availability badge — only shown if personal.available is True in app.py -->
      
      <div class="hero-badge">
        <span class="badge-dot"></span>
        <!-- typedRole gets filled by the typing animation JavaScript below -->
        <span id="typedRole"></span><span class="typed-cursor">|</span>
      </div>
      

      <h1 class="hero-title">
        <span class="line1">Crafting Digital</span>
        <span class="highlight">Universes.</span>
      </h1>

      <p class="hero-sub">
        I build beautiful, high-performance web experiences — living at the intersection of
        <strong style="color:var(--blue)">design</strong>,
        <strong style="color:var(--cyan)">engineering</strong>, and
        <strong style="color:var(--green)">innovation</strong>.
      </p>

      <div class="hero-actions">
        <a href="#projects" class="btn-primary">
          View My Work <span>→</span>
        </a>
        <a href="#contact" class="btn-outline">
          Let's Talk
        </a>
      </div>

      <!-- Stats row — data-count is read by JavaScript to animate the counter -->
      <div class="hero-stats">
        <div class="stat">
          <div class="stat-num" data-count="42">0</div>
          <div class="stat-label">Projects Shipped</div>
        </div>
        <div class="stat">
          <div class="stat-num" data-count="6">0</div>
          <div class="stat-label">Years Experience</div>
        </div>
        <div class="stat">
          <div class="stat-num" data-count="18">0</div>
          <div class="stat-label">Happy Clients</div>
        </div>
      </div>
    </div>

    <!-- Floating code decorations — purely visual, positioned in CSS -->
    <div class="float-code" style="top:18%;right:10%">const universe = await build();</div>
    <div class="float-code" style="top:33%;right:24%">{ stars: ∞, bugs: 0 }</div>
    <div class="float-code" style="bottom:26%;right:9%">npm run deploy --cosmos</div>
    <div class="float-code" style="bottom:40%;right:28%">02:47 ◆ All systems nominal</div>
  </section>

  <!-- =====================================================
       ABOUT SECTION
  ===================================================== -->
  <section id="about">
    <!-- .reveal class = starts hidden, becomes visible on scroll (via IntersectionObserver) -->
    <div class="section-label reveal">// about.me</div>
    <div class="section-title reveal">I'm a <span class="accent">Full-Stack</span><br>Builder & Thinker</div>
    <div class="section-divider reveal"></div>

    <div class="about-grid">
      <!-- Left: text paragraphs -->
      <div class="about-text reveal">
        <p>
          Hey — I'm <strong>Your Name</strong>, a product engineer obsessed with
          <strong>making complex things feel effortless</strong>. I've spent 6+ years
          shipping software that scales — from zero-to-one startups to enterprise
          platforms serving millions.
        </p>
        <p>
          I love the full stack: pixel-perfect frontend, robust APIs, distributed systems,
          and the <strong>creative strategy</strong> that ties it all together.
          I don't just write code — I solve real problems.
        </p>
        <p>
          When I'm not in the terminal, I'm exploring <strong>AI/ML research</strong>,
          contributing to open source, or staring at the night sky thinking about
          orbital mechanics. 🌌
        </p>

        <!-- Info chips: location, availability, etc. -->
        <div class="about-chips">
          <div class="chip"><span>📍</span> San Francisco, CA</div>
          
          <div class="chip"><span>✅</span> Open to Work</div>
          
          <div class="chip"><span>🚀</span> Full-Stack</div>
          <div class="chip"><span>🤖</span> AI/ML</div>
        </div>
      </div>

      <!-- Right: skill bars
           Jinja2 for loop: iterates over the `skills` list passed from app.py -->
      <div class="skills-grid reveal">
        
        <div class="skill-item">
          <div class="skill-header">
            <span>Python</span>
            <!-- skill.level is the number (e.g. 95) from SKILLS in app.py -->
            <span class="skill-pct">95%</span>
          </div>
          <div class="skill-bar">
            <!-- data-level is read by JavaScript to set the bar's width -->
            <div class="skill-fill" data-level="95"></div>
          </div>
        </div>
        
        <div class="skill-item">
          <div class="skill-header">
            <span>JavaScript / TS</span>
            <!-- skill.level is the number (e.g. 95) from SKILLS in app.py -->
            <span class="skill-pct">92%</span>
          </div>
          <div class="skill-bar">
            <!-- data-level is read by JavaScript to set the bar's width -->
            <div class="skill-fill" data-level="92"></div>
          </div>
        </div>
        
        <div class="skill-item">
          <div class="skill-header">
            <span>React / Next.js</span>
            <!-- skill.level is the number (e.g. 95) from SKILLS in app.py -->
            <span class="skill-pct">90%</span>
          </div>
          <div class="skill-bar">
            <!-- data-level is read by JavaScript to set the bar's width -->
            <div class="skill-fill" data-level="90"></div>
          </div>
        </div>
        
        <div class="skill-item">
          <div class="skill-header">
            <span>Machine Learning</span>
            <!-- skill.level is the number (e.g. 95) from SKILLS in app.py -->
            <span class="skill-pct">88%</span>
          </div>
          <div class="skill-bar">
            <!-- data-level is read by JavaScript to set the bar's width -->
            <div class="skill-fill" data-level="88"></div>
          </div>
        </div>
        
        <div class="skill-item">
          <div class="skill-header">
            <span>Node.js</span>
            <!-- skill.level is the number (e.g. 95) from SKILLS in app.py -->
            <span class="skill-pct">87%</span>
          </div>
          <div class="skill-bar">
            <!-- data-level is read by JavaScript to set the bar's width -->
            <div class="skill-fill" data-level="87"></div>
          </div>
        </div>
        
        <div class="skill-item">
          <div class="skill-header">
            <span>Docker / K8s</span>
            <!-- skill.level is the number (e.g. 95) from SKILLS in app.py -->
            <span class="skill-pct">83%</span>
          </div>
          <div class="skill-bar">
            <!-- data-level is read by JavaScript to set the bar's width -->
            <div class="skill-fill" data-level="83"></div>
          </div>
        </div>
        
        <div class="skill-item">
          <div class="skill-header">
            <span>PostgreSQL</span>
            <!-- skill.level is the number (e.g. 95) from SKILLS in app.py -->
            <span class="skill-pct">85%</span>
          </div>
          <div class="skill-bar">
            <!-- data-level is read by JavaScript to set the bar's width -->
            <div class="skill-fill" data-level="85"></div>
          </div>
        </div>
        
        <div class="skill-item">
          <div class="skill-header">
            <span>Go / Rust</span>
            <!-- skill.level is the number (e.g. 95) from SKILLS in app.py -->
            <span class="skill-pct">75%</span>
          </div>
          <div class="skill-bar">
            <!-- data-level is read by JavaScript to set the bar's width -->
            <div class="skill-fill" data-level="75"></div>
          </div>
        </div>
        
      </div>
    </div>
  </section>

  <!-- =====================================================
       PROJECTS SECTION
  ===================================================== -->
  <section id="projects">
    <div class="section-label reveal">// projects.featured</div>
    <div class="section-title reveal">Things I've <span class="accent">Built</span></div>
    <p class="section-desc reveal">A selection of projects that push boundaries — each one a small universe of its own.</p>

    <!-- reveal-stagger = children appear one after another with CSS delays -->
    <div class="projects-grid reveal-stagger">

      <!-- Jinja2 loop over the PROJECTS list from app.py -->
      
      <div class="project-card reveal"
           data-color="#0ea5e9"
           style="--card-accent: #0ea5e9;">

        <!-- Card header row -->
        <div class="card-top">
          <div class="card-icon">🧠</div>

          <!-- Conditionally apply status class based on project.status value.
               Jinja2 if/elif/else mirrors Python's if/elif/else -->
          <div class="card-status
            status-live
            ">
            Live
          </div>
        </div>

        <div class="card-category">Artificial Intelligence</div>
        <div class="card-title">Nebula AI</div>
        <div class="card-desc">A deep learning platform that predicts user behavior patterns using transformer-based neural networks trained on multi-modal data.</div>

        <!-- Tech stack tags — another inner loop -->
        <div class="card-tech">
          
          <span class="tech-tag">Python</span>
          
          <span class="tech-tag">PyTorch</span>
          
          <span class="tech-tag">FastAPI</span>
          
          <span class="tech-tag">Docker</span>
          
        </div>

        <div class="card-footer">
          <span class="card-year">2024</span>
          <!-- Link to the project — project.link from app.py -->
          <a href="#" class="card-arrow" target="_blank" rel="noopener">↗</a>
        </div>
      </div>
      
      <div class="project-card reveal"
           data-color="#06b6d4"
           style="--card-accent: #06b6d4;">

        <!-- Card header row -->
        <div class="card-top">
          <div class="card-icon">📡</div>

          <!-- Conditionally apply status class based on project.status value.
               Jinja2 if/elif/else mirrors Python's if/elif/else -->
          <div class="card-status
            status-live
            ">
            Live
          </div>
        </div>

        <div class="card-category">Full Stack Web</div>
        <div class="card-title">Orion Dashboard</div>
        <div class="card-desc">Real-time analytics dashboard with WebSocket streams, 3D data visualizations, and ML-powered anomaly detection for enterprise scale.</div>

        <!-- Tech stack tags — another inner loop -->
        <div class="card-tech">
          
          <span class="tech-tag">React</span>
          
          <span class="tech-tag">Node.js</span>
          
          <span class="tech-tag">D3.js</span>
          
          <span class="tech-tag">PostgreSQL</span>
          
        </div>

        <div class="card-footer">
          <span class="card-year">2024</span>
          <!-- Link to the project — project.link from app.py -->
          <a href="#" class="card-arrow" target="_blank" rel="noopener">↗</a>
        </div>
      </div>
      
      <div class="project-card reveal"
           data-color="#3b82f6"
           style="--card-accent: #3b82f6;">

        <!-- Card header row -->
        <div class="card-top">
          <div class="card-icon">💎</div>

          <!-- Conditionally apply status class based on project.status value.
               Jinja2 if/elif/else mirrors Python's if/elif/else -->
          <div class="card-status
            status-beta
            ">
            Beta
          </div>
        </div>

        <div class="card-category">Fintech</div>
        <div class="card-title">Pulsar Finance</div>
        <div class="card-desc">Blockchain-powered DeFi protocol enabling cross-chain asset swaps with zero slippage using AMM liquidity optimization algorithms.</div>

        <!-- Tech stack tags — another inner loop -->
        <div class="card-tech">
          
          <span class="tech-tag">Solidity</span>
          
          <span class="tech-tag">Web3.js</span>
          
          <span class="tech-tag">Next.js</span>
          
          <span class="tech-tag">Rust</span>
          
        </div>

        <div class="card-footer">
          <span class="card-year">2023</span>
          <!-- Link to the project — project.link from app.py -->
          <a href="#" class="card-arrow" target="_blank" rel="noopener">↗</a>
        </div>
      </div>
      
      <div class="project-card reveal"
           data-color="#22d3ee"
           style="--card-accent: #22d3ee;">

        <!-- Card header row -->
        <div class="card-top">
          <div class="card-icon">🌌</div>

          <!-- Conditionally apply status class based on project.status value.
               Jinja2 if/elif/else mirrors Python's if/elif/else -->
          <div class="card-status
            status-live
            ">
            Live
          </div>
        </div>

        <div class="card-category">Developer Tools</div>
        <div class="card-title">Cosmos CMS</div>
        <div class="card-desc">Headless CMS with visual builder, AI content generation, and edge-first deployment — zero config, infinite scale.</div>

        <!-- Tech stack tags — another inner loop -->
        <div class="card-tech">
          
          <span class="tech-tag">TypeScript</span>
          
          <span class="tech-tag">GraphQL</span>
          
          <span class="tech-tag">Redis</span>
          
          <span class="tech-tag">Cloudflare</span>
          
        </div>

        <div class="card-footer">
          <span class="card-year">2023</span>
          <!-- Link to the project — project.link from app.py -->
          <a href="#" class="card-arrow" target="_blank" rel="noopener">↗</a>
        </div>
      </div>
      
      <div class="project-card reveal"
           data-color="#0284c7"
           style="--card-accent: #0284c7;">

        <!-- Card header row -->
        <div class="card-top">
          <div class="card-icon">⚡</div>

          <!-- Conditionally apply status class based on project.status value.
               Jinja2 if/elif/else mirrors Python's if/elif/else -->
          <div class="card-status
            status-live
            ">
            Live
          </div>
        </div>

        <div class="card-category">Mobile App</div>
        <div class="card-title">Vega Mobile</div>
        <div class="card-desc">Cross-platform fitness app with computer vision form correction, adaptive workout AI, and social competition mechanics.</div>

        <!-- Tech stack tags — another inner loop -->
        <div class="card-tech">
          
          <span class="tech-tag">Flutter</span>
          
          <span class="tech-tag">TensorFlow Lite</span>
          
          <span class="tech-tag">Firebase</span>
          
          <span class="tech-tag">Swift</span>
          
        </div>

        <div class="card-footer">
          <span class="card-year">2024</span>
          <!-- Link to the project — project.link from app.py -->
          <a href="#" class="card-arrow" target="_blank" rel="noopener">↗</a>
        </div>
      </div>
      
      <div class="project-card reveal"
           data-color="#0369a1"
           style="--card-accent: #0369a1;">

        <!-- Card header row -->
        <div class="card-top">
          <div class="card-icon">🚀</div>

          <!-- Conditionally apply status class based on project.status value.
               Jinja2 if/elif/else mirrors Python's if/elif/else -->
          <div class="card-status
            status-open">
            Open Source
          </div>
        </div>

        <div class="card-category">Infrastructure</div>
        <div class="card-title">Aurora DevOps</div>
        <div class="card-desc">GitOps-native deployment platform with automatic rollbacks, canary releases, and intelligent traffic splitting across clouds.</div>

        <!-- Tech stack tags — another inner loop -->
        <div class="card-tech">
          
          <span class="tech-tag">Kubernetes</span>
          
          <span class="tech-tag">Terraform</span>
          
          <span class="tech-tag">Go</span>
          
          <span class="tech-tag">Prometheus</span>
          
        </div>

        <div class="card-footer">
          <span class="card-year">2023</span>
          <!-- Link to the project — project.link from app.py -->
          <a href="https://github.com" class="card-arrow" target="_blank" rel="noopener">↗</a>
        </div>
      </div>
      

    </div>
  </section>

  <!-- =====================================================
       CONTACT SECTION
       The form uses JavaScript fetch() to POST to /contact.
       No page reload — the response is shown as a toast.
  ===================================================== -->
  <section id="contact">
    <div class="section-label reveal">// contact.init()</div>

    <div class="contact-grid">

      <!-- LEFT: The actual form -->
      <div class="contact-form-wrap reveal">
        <div class="form-title">Send a Message</div>
        <p class="form-subtitle">Got a project in mind? I'd love to hear about it. Fill in the form and I'll get back to you within 24 hours.</p>

        <!-- id="contactForm" is referenced by JavaScript below.
             We don't use the <form> action attribute — JS intercepts the submit. -->
        <div id="contactForm">

          <!-- Name + Email row (2 columns) -->
          <div class="form-row">
            <div class="form-group">
              <label class="form-label" for="inputName">Your Name</label>
              <input class="form-input" type="text" id="inputName" placeholder="Alex Johnson" required>
            </div>
            <div class="form-group">
              <label class="form-label" for="inputEmail">Email Address</label>
              <input class="form-input" type="email" id="inputEmail" placeholder="alex@company.com" required>
            </div>
          </div>

          <!-- Subject -->
          <div class="form-group">
            <label class="form-label" for="inputSubject">Subject</label>
            <input class="form-input" type="text" id="inputSubject" placeholder="Project proposal / Collaboration / Just saying hi...">
          </div>

          <!-- Message -->
          <div class="form-group">
            <label class="form-label" for="inputMessage">Message</label>
            <textarea class="form-input" id="inputMessage" rows="5" placeholder="Tell me about your project, idea, or just say hello. I read every message."></textarea>
          </div>

          <!-- Submit button — onclick calls submitForm() defined in JavaScript -->
          <button class="form-submit" onclick="submitForm()">
            <span id="btnText">Send Message</span>
            <!-- Spinner is hidden by default; shown during loading -->
            <div class="spinner" id="submitSpinner"></div>
            <span id="btnIcon">🚀</span>
          </button>

          <!-- Toast notification shown after form submission -->
          <div class="form-toast" id="formToast"></div>
        </div>
      </div>

      <!-- RIGHT: Contact info column -->
      <div class="contact-info reveal">
        <div class="contact-info-title">
          Let's build<br><span style="color:var(--blue)">something great.</span>
        </div>
        <p class="contact-info-desc">
          I'm currently available for freelance projects, full-time roles, and ambitious ideas.
          Whether you have a clear brief or just a spark — I'd love to talk.
        </p>

        <!-- Email detail -->
        <div class="contact-detail">
          <div class="contact-detail-icon">✉️</div>
          <div>
            <div class="contact-detail-label">Email</div>
            <!-- personal.email comes from PERSONAL dict in app.py -->
            <div class="contact-detail-value">
              <a href="mailto:hello@yourportfolio.dev">hello@yourportfolio.dev</a>
            </div>
          </div>
        </div>

        <!-- Location detail -->
        <div class="contact-detail">
          <div class="contact-detail-icon">📍</div>
          <div>
            <div class="contact-detail-label">Location</div>
            <div class="contact-detail-value">San Francisco, CA</div>
          </div>
        </div>

        <!-- Availability status -->
        <div class="contact-detail">
          <div class="contact-detail-icon">⚡</div>
          <div>
            <div class="contact-detail-label">Status</div>
            <div class="contact-detail-value" style="color:var(--green)">
              Available for new projects
            </div>
          </div>
        </div>

        <!-- Social links row -->
        <div class="contact-socials">
          <a href="https://github.com"   class="social-link" target="_blank">⌥ GitHub</a>
          <a href="https://linkedin.com" class="social-link" target="_blank">in LinkedIn</a>
          <a href="https://twitter.com"  class="social-link" target="_blank">✕ Twitter</a>
        </div>
      </div>
    </div>
  </section>

  <!-- =====================================================
       FOOTER
  ===================================================== -->
  <footer>
    <div class="footer-copy">
      Crafted with <span>♥</span> + Flask + ☕ &nbsp;·&nbsp; © 2025 Your Name
    </div>
    <div class="footer-copy">
      <span>Built in Python</span> &nbsp;·&nbsp; Deployed with love
    </div>
  </footer>


  <!-- ===========================================================
       JAVASCRIPT
       All JS is in one <script> block at the bottom of <body>.
       Placing it at the bottom ensures the HTML is parsed first,
       so elements like #cursor and #solarCanvas exist when JS runs.
  =========================================================== -->
  <script>
  // =============================================================
  // SECTION 1: SOLAR SYSTEM CANVAS ANIMATION
  // We use the HTML5 Canvas API to draw and animate the solar system.
  // Every frame: clear → draw stars → draw orbits → draw sun → draw planets
  // =============================================================

  // Get the canvas element and its 2D drawing context
  // ctx is the object we call drawing methods on (ctx.arc, ctx.fillStyle, etc.)
  const canvas = document.getElementById('solarCanvas');
  const ctx    = canvas.getContext('2d');

  // Track canvas dimensions and scroll progress
  let W, H;
  let scrollProgress = 0; // 0 = top of page, 1 = bottom of page

  // resize() updates W and H to match the browser window.
  // Called once on load, then again whenever the window resizes.
  function resize() {
    W = canvas.width  = window.innerWidth;
    H = canvas.height = window.innerHeight;
  }
  resize();
  window.addEventListener('resize', resize);

  // Track scroll position as a 0–1 value.
  // This is used to speed up planet orbits when scrolling.
  window.addEventListener('scroll', () => {
    const maxScroll = document.body.scrollHeight - window.innerHeight;
    scrollProgress = maxScroll > 0 ? window.scrollY / maxScroll : 0;
  });

  // Helper: is the current theme dark? Reads the data-theme attribute.
  const isDark = () => document.documentElement.getAttribute('data-theme') !== 'light';

  // Sun position — placed at ~70% across and 45% down
  const sunX = () => W * 0.70;
  const sunY = () => H * 0.44;

  // Planet definitions — each object describes one planet's properties.
  // To add a planet: copy a block, change the values.
  const planets = [
    {
      name:  'Mercury',
      r:     5,            // radius in pixels
      dist:  68,           // orbital radius (distance from sun center)
      speed: 4.2,          // orbital speed multiplier
      color: '#a0a0a0',    // planet fill color
      angle: 0.4           // initial starting angle (radians)
    },
    {
      name: 'Venus', r: 9, dist: 108, speed: 2.7, color: '#e8c97a', angle: 1.3
    },
    {
      name:  'Earth',
      r:     10, dist: 158, speed: 2.0, color: '#4fc3f7', angle: 2.2,
      // Moon: a smaller body orbiting Earth
      moon: { r: 3, dist: 21, speed: 8.5, color: '#9ca3af', angle: 0.5 }
    },
    {
      name: 'Mars', r: 7, dist: 212, speed: 1.6, color: '#ef7c52', angle: 0.9
    },
    {
      name:  'Jupiter',
      r:     19, dist: 295, speed: 0.9, color: '#c9956e', angle: 3.6,
      rings: true  // Draw a ring around this planet
    },
    {
      name:  'Saturn',
      r:     15, dist: 380, speed: 0.6, color: '#e8d5a3', angle: 2.0,
      rings: true, ringColor: 'rgba(232,200,120,0.4)'
    },
    {
      name: 'Uranus', r: 11, dist: 460, speed: 0.38, color: '#7de8e8', angle: 4.3
    },
  ];

  // Generate stars once — random positions, sizes, and twinkle offsets.
  // We store them as fractions (x: 0–1, y: 0–1) so they work at any canvas size.
  const stars = Array.from({ length: 300 }, () => ({
    x:       Math.random(),          // 0 to 1 fraction of canvas width
    y:       Math.random(),          // 0 to 1 fraction of canvas height
    r:       Math.random() * 1.4 + 0.2,  // radius: 0.2px to 1.6px
    opacity: Math.random() * 0.65 + 0.2, // base opacity
    twinkle: Math.random() * Math.PI * 2 // phase offset so they don't all twinkle in sync
  }));

  // t = elapsed time in seconds (passed to requestAnimationFrame callback)
  // Used to drive all time-based animations
  let t = 0;

  // --- drawStars: render all stars with a gentle twinkle ---
  function drawStars() {
    stars.forEach(s => {
      // sin() oscillates between -1 and 1; we map it to 0.3–1.0 for subtle twinkle
      const twinkle = 0.35 + 0.65 * Math.abs(Math.sin(s.twinkle + t * 0.4));
      ctx.beginPath();
      ctx.arc(s.x * W, s.y * H, s.r, 0, Math.PI * 2);
      ctx.fillStyle = isDark()
        ? `rgba(200,220,255,${s.opacity * twinkle})`   // Blue-white stars in dark mode
        : `rgba(80,120,200,${s.opacity * 0.22 * twinkle})`; // Faint in light mode
      ctx.fill();
    });
  }

  // --- drawOrbit: draw a faint ellipse for a planet's orbital path ---
  function drawOrbit(cx, cy, dist) {
    ctx.beginPath();
    // Ellipse: same center, horizontal radius = dist, vertical = dist*0.38 (foreshortened)
    ctx.ellipse(cx, cy, dist, dist * 0.38, 0, 0, Math.PI * 2);
    ctx.strokeStyle = isDark()
      ? 'rgba(100,160,255,0.07)'
      : 'rgba(14,100,200,0.06)';
    ctx.lineWidth = 1;
    ctx.stroke();
  }

  // --- drawPlanet: calculate position and draw one planet ---
  function drawPlanet(planet, cx, cy, elapsed) {
    // Compute orbit angle at this moment.
    // scrollProgress speeds up animation as user scrolls down.
    const speed = planet.speed * (0.28 + scrollProgress * 2.8);
    const angle = planet.angle + elapsed * speed;

    // Convert polar coordinates (angle + distance) to Cartesian (x, y).
    // We multiply y by 0.38 to make it look like a 3D tilted orbit.
    const px = cx + Math.cos(angle) * planet.dist;
    const py = cy + Math.sin(angle) * planet.dist * 0.38;

    // --- Draw rings (Saturn / Jupiter) ---
    if (planet.rings) {
      ctx.save();                    // Save transform state
      ctx.translate(px, py);         // Move origin to planet center
      ctx.scale(1, 0.28);            // Squish vertically for ring perspective
      ctx.beginPath();
      ctx.arc(0, 0, planet.r * 2.2, 0, Math.PI * 2); // Ring arc
      ctx.strokeStyle = planet.ringColor || 'rgba(200,180,120,0.3)';
      ctx.lineWidth = planet.r * 0.65;
      ctx.stroke();
      ctx.restore();                 // Restore transform so nothing else is squished
    }

    // --- Draw planet glow (soft radial gradient halo) ---
    const grd = ctx.createRadialGradient(px, py, 0, px, py, planet.r * 2.8);
    grd.addColorStop(0, planet.color + '99');  // Semi-transparent center
    grd.addColorStop(1, 'transparent');
    ctx.beginPath();
    ctx.arc(px, py, planet.r * 2.8, 0, Math.PI * 2);
    ctx.fillStyle = grd;
    ctx.fill();

    // --- Draw planet body ---
    ctx.beginPath();
    ctx.arc(px, py, planet.r, 0, Math.PI * 2);
    ctx.fillStyle = planet.color;
    ctx.fill();

    // --- Draw moon (Earth only) ---
    if (planet.moon) {
      const m = planet.moon;
      const ma = m.angle + elapsed * m.speed; // Moon orbits faster than planet
      const mx = px + Math.cos(ma) * m.dist;
      const my = py + Math.sin(ma) * m.dist * 0.5; // Same vertical squish factor
      ctx.beginPath();
      ctx.arc(mx, my, m.r, 0, Math.PI * 2);
      ctx.fillStyle = m.color;
      ctx.fill();
    }
  }

  // --- drawSun: the central star with corona glow layers ---
  function drawSun(cx, cy) {
    // Draw 3 concentric glow rings (outer corona)
    for (let i = 3; i >= 1; i--) {
      const grd = ctx.createRadialGradient(cx, cy, 0, cx, cy, 44 * i);
      grd.addColorStop(0, `rgba(255,200,60,${0.07 / i})`);
      grd.addColorStop(1, 'transparent');
      ctx.beginPath();
      ctx.arc(cx, cy, 44 * i, 0, Math.PI * 2);
      ctx.fillStyle = grd;
      ctx.fill();
    }

    // Draw the sun body with a warm gradient (bright core → orange edge)
    const sg = ctx.createRadialGradient(cx - 6, cy - 6, 0, cx, cy, 26);
    sg.addColorStop(0,   '#fffbe0');  // Bright white-yellow center
    sg.addColorStop(0.3, '#ffd766');  // Yellow
    sg.addColorStop(0.7, '#ff9e1a');  // Orange
    sg.addColorStop(1,   '#e05500');  // Deep orange edge
    ctx.beginPath();
    ctx.arc(cx, cy, 24, 0, Math.PI * 2);
    ctx.fillStyle = sg;
    ctx.fill();
  }

  // --- Main animation loop ---
  // requestAnimationFrame(frame) tells the browser to call frame() before the next repaint.
  // ts = timestamp in milliseconds provided by the browser
  function frame(ts) {
    requestAnimationFrame(frame); // Schedule the next frame (creates the loop)

    t = ts * 0.001; // Convert ms to seconds for readable speed values

    // 1. Clear the entire canvas
    ctx.clearRect(0, 0, W, H);

    // 2. Draw background stars
    drawStars();

    const cx = sunX(), cy = sunY(); // Recalculate sun position in case window resized

    // 3. Draw orbital paths (faint ellipses)
    planets.forEach(p => drawOrbit(cx, cy, p.dist));

    // 4. Draw the sun
    drawSun(cx, cy);

    // 5. Draw each planet on top
    planets.forEach(p => drawPlanet(p, cx, cy, t));
  }

  // Start the animation loop
  requestAnimationFrame(frame);


  // =============================================================
  // SECTION 2: CUSTOM CURSOR
  // Two elements — a small dot that follows instantly,
  // and a larger ring that "lerps" (lerps = linear interpolation)
  // toward the mouse position for a smooth trailing effect.
  // =============================================================

  const cursorDot  = document.getElementById('cursor');
  const cursorRing = document.getElementById('cursorRing');

  // mx, my = actual mouse position (updates instantly)
  // rx, ry = ring position (updates slowly by lerping toward mx/my)
  let mx = 0, my = 0, rx = 0, ry = 0;

  // Update dot position instantly on mouse move
  document.addEventListener('mousemove', e => {
    mx = e.clientX;
    my = e.clientY;
    // Move dot directly — no delay
    cursorDot.style.left = mx + 'px';
    cursorDot.style.top  = my + 'px';
  });

  // Animate the ring with lag using requestAnimationFrame
  // Lerping formula: current += (target - current) * factor
  //   factor = 0.12 means "move 12% of remaining distance each frame"
  //   Smaller factor = more lag, larger factor = less lag
  (function animateRing() {
    rx += (mx - rx) * 0.12;  // Lerp X toward mouse
    ry += (my - ry) * 0.12;  // Lerp Y toward mouse
    cursorRing.style.left = rx + 'px';
    cursorRing.style.top  = ry + 'px';
    requestAnimationFrame(animateRing);
  })();


  // =============================================================
  // SECTION 3: 3D TILT EFFECT ON PROJECT CARDS
  // On mousemove inside a card:
  //   - Calculate how far the mouse is from the card center (dx, dy)
  //   - Apply a perspective rotation using CSS transform
  //   - Move the glowing ::after overlay to the mouse position
  // On mouseleave: reset everything back to normal
  // =============================================================

  document.querySelectorAll('.project-card').forEach(card => {

    card.addEventListener('mousemove', e => {
      const rect = card.getBoundingClientRect(); // Card's size and position

      // Center of the card in screen coordinates
      const centerX = rect.left + rect.width  / 2;
      const centerY = rect.top  + rect.height / 2;

      // Normalized distance: -1 (far left) to +1 (far right)
      const dx = (e.clientX - centerX) / (rect.width  / 2);
      const dy = (e.clientY - centerY) / (rect.height / 2);

      // Rotation angles — multiply by max tilt degrees (12)
      // rotateX: tilts top/bottom (positive = tilt away from top)
      // rotateY: tilts left/right (positive = tilt toward right)
      const rotX = -dy * 12; // Invert Y so top-hover tilts card top away
      const rotY =  dx * 12;

      // Apply 3D transform
      card.style.transform =
        `perspective(700px) rotateX(${rotX}deg) rotateY(${rotY}deg) scale(1.025) translateZ(8px)`;

      // Dynamic shadow that shifts based on tilt direction
      card.style.boxShadow =
        `${-dx * 20}px ${-dy * 20}px 50px rgba(14,165,233,0.16), 0 24px 64px rgba(0,0,0,0.35)`;

      // Update the CSS custom properties used by the ::after glow gradient.
      // These represent mouse position as a percentage of the card's dimensions.
      const pctX = ((e.clientX - rect.left) / rect.width  * 100).toFixed(1) + '%';
      const pctY = ((e.clientY - rect.top)  / rect.height * 100).toFixed(1) + '%';
      card.style.setProperty('--mx', pctX);
      card.style.setProperty('--my', pctY);
    });

    // Reset all transforms and shadows when mouse leaves the card
    card.addEventListener('mouseleave', () => {
      card.style.transform  = '';
      card.style.boxShadow  = '';
    });
  });


  // =============================================================
  // SECTION 4: THEME TOGGLE (Dark / Light Mode)
  // Toggles the data-theme attribute on <html>.
  // CSS uses [data-theme="light"] { ... } to override variables.
  // =============================================================

  let isLight = false; // Track current theme state

  document.getElementById('themeToggle').addEventListener('click', () => {
    isLight = !isLight;
    // Set the attribute that CSS watches for
    document.documentElement.setAttribute('data-theme', isLight ? 'light' : 'dark');
    // Update the button icon to reflect the current theme
    document.getElementById('themeToggle').textContent = isLight ? '☀️' : '🌙';
  });


  // =============================================================
  // SECTION 5: SCROLL REVEAL
  // IntersectionObserver watches elements with class .reveal.
  // When an element enters the viewport, it adds class .visible,
  // which triggers a CSS transition (opacity + translateY).
  // =============================================================

  // IntersectionObserver(callback, options)
  // callback runs when observed elements enter/leave the viewport
  // threshold: 0.12 = trigger when 12% of the element is visible
  const revealObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry, i) => {
      if (entry.isIntersecting) {
        // Stagger: each element appears 80ms after the previous one
        setTimeout(() => {
          entry.target.classList.add('visible');
        }, i * 80);
        // Once revealed, we stop observing (no need to re-trigger)
        revealObserver.unobserve(entry.target);
      }
    });
  }, { threshold: 0.12 });

  // Observe all elements with the .reveal class
  document.querySelectorAll('.reveal').forEach(el => revealObserver.observe(el));


  // =============================================================
  // SECTION 6: SKILL BAR ANIMATION
  // When the skills section scrolls into view:
  // JavaScript sets each .skill-fill's width to its data-level value.
  // The CSS transition: width 1.4s then animates it smoothly.
  // =============================================================

  const skillObserver = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        // Find all skill bars inside this section
        entry.target.querySelectorAll('.skill-fill').forEach((fill, i) => {
          // Small staggered delay so each bar animates in sequence
          setTimeout(() => {
            fill.style.width = fill.dataset.level + '%';
          }, i * 120);
        });
        skillObserver.unobserve(entry.target); // Animate only once
      }
    });
  }, { threshold: 0.25 });

  const skillsGrid = document.querySelector('.skills-grid');
  if (skillsGrid) skillObserver.observe(skillsGrid);


  // =============================================================
  // SECTION 7: COUNTER ANIMATION
  // When the hero stats scroll into view, each number counts
  // up from 0 to its data-count value.
  // =============================================================

  const counterObserver = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        // Find every element with a data-count attribute inside hero-stats
        entry.target.querySelectorAll('[data-count]').forEach(el => {
          const target = parseInt(el.dataset.count); // The final number to count to
          let current = 0;
          const step = Math.ceil(target / 45); // How much to add per tick

          // setInterval repeatedly calls the function every 30ms
          const timer = setInterval(() => {
            current = Math.min(current + step, target); // Don't exceed target
            el.textContent = current + (target >= 10 ? '+' : ''); // Append "+" for larger numbers
            if (current >= target) clearInterval(timer); // Stop when done
          }, 30);
        });
        counterObserver.unobserve(entry.target);
      }
    });
  }, { threshold: 0.5 });

  const heroStats = document.querySelector('.hero-stats');
  if (heroStats) counterObserver.observe(heroStats);


  // =============================================================
  // SECTION 8: TYPING ANIMATION
  // Cycles through a list of role titles, typing and deleting
  // each one character by character, like a terminal cursor.
  // =============================================================

  const roles = [
    'Full-Stack Engineer',
    'AI / ML Builder',
    'UI/UX Craftsman',
    'Open Source Creator',
    'Digital Architect',
  ];

  let roleIndex  = 0;    // Which role are we currently showing?
  let charIndex  = 0;    // How many characters are currently visible?
  let isDeleting = false; // Are we typing forward or deleting backward?

  const typedEl = document.getElementById('typedRole');

  function typeNext() {
    if (!typedEl) return; // Guard: element might not exist if badge is hidden

    const currentRole = roles[roleIndex];

    if (!isDeleting) {
      // ADD one character
      typedEl.textContent = currentRole.slice(0, charIndex + 1);
      charIndex++;

      if (charIndex === currentRole.length) {
        // Finished typing — pause before starting to delete
        isDeleting = true;
        setTimeout(typeNext, 2000); // Wait 2 seconds before deleting
        return;
      }
    } else {
      // REMOVE one character
      typedEl.textContent = currentRole.slice(0, charIndex - 1);
      charIndex--;

      if (charIndex === 0) {
        // Finished deleting — move to next role
        isDeleting = false;
        roleIndex  = (roleIndex + 1) % roles.length; // Wrap around the array
      }
    }

    // Schedule the next character
    // Deleting is faster (40ms) than typing (85ms) — feels natural
    setTimeout(typeNext, isDeleting ? 40 : 85);
  }

  typeNext(); // Kick off the typing animation


  // =============================================================
  // SECTION 9: CONTACT FORM SUBMISSION
  // Uses the Fetch API to send form data as JSON to Flask's
  // /contact route WITHOUT refreshing the page.
  // This is called "AJAX" (Asynchronous JavaScript and XML — though we use JSON).
  // =============================================================

  async function submitForm() {
    // --- Gather form values ---
    const name    = document.getElementById('inputName').value.trim();
    const email   = document.getElementById('inputEmail').value.trim();
    const subject = document.getElementById('inputSubject').value.trim() || 'No subject';
    const message = document.getElementById('inputMessage').value.trim();

    // --- Frontend validation ---
    // We validate here for immediate feedback, but Flask also validates on the server.
    if (!name || !email || !message) {
      showToast('Please fill in your name, email, and message.', 'error');
      return;
    }
    if (!email.includes('@')) {
      showToast('Please enter a valid email address.', 'error');
      return;
    }

    // --- Show loading state ---
    const btn     = document.querySelector('.form-submit');
    const btnText = document.getElementById('btnText');
    const spinner = document.getElementById('submitSpinner');
    const btnIcon = document.getElementById('btnIcon');

    btn.disabled         = true;           // Prevent double-submission
    btnText.textContent  = 'Sending...';
    spinner.style.display = 'block';        // Show spinning loader
    btnIcon.style.display = 'none';         // Hide rocket emoji

    try {
      // --- Send POST request to Flask ---
      // fetch(url, options) is the modern way to make HTTP requests from JavaScript.
      const response = await fetch('/contact', {
        method: 'POST',                          // HTTP method must match Flask's methods=['POST']
        headers: { 'Content-Type': 'application/json' }, // Tell server we're sending JSON
        body: JSON.stringify({ name, email, subject, message }) // Convert JS object to JSON string
      });

      // Parse the JSON response from Flask
      const data = await response.json();

      if (data.success) {
        // --- Success ---
        showToast(data.message, 'success');
        // Clear all form inputs
        ['inputName', 'inputEmail', 'inputSubject', 'inputMessage'].forEach(id => {
          document.getElementById(id).value = '';
        });
        btnText.textContent = 'Message Sent! ✓';
      } else {
        // --- Server returned an error ---
        showToast(data.error || 'Something went wrong. Please try again.', 'error');
        btnText.textContent = 'Try Again';
      }

    } catch (err) {
      // --- Network error (e.g. server is down) ---
      console.error('Fetch error:', err);
      showToast('Network error. Please check your connection.', 'error');
      btnText.textContent = 'Try Again';
    } finally {
      // 'finally' runs whether the request succeeded or failed
      spinner.style.display = 'none';
      btnIcon.style.display = 'inline';
      btn.disabled = false;
    }
  }

  // --- showToast: display a notification message below the form ---
  // type = 'success' (green) or 'error' (red)
  function showToast(message, type) {
    const toast = document.getElementById('formToast');
    toast.textContent = message;
    toast.className   = `form-toast ${type}`; // Apply color class
    toast.style.display = 'block';

    // Auto-hide the toast after 5 seconds
    setTimeout(() => { toast.style.display = 'none'; }, 5000);
  }

  // Allow pressing Enter in the message textarea to submit
  // (Shift+Enter still adds a new line — only plain Enter submits)
  document.getElementById('inputMessage')?.addEventListener('keydown', e => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault(); // Don't add a newline
      submitForm();
    }
  });

  </script>
</body>
</html>
