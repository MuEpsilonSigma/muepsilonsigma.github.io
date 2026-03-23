---
title: Projects - Joon
layout: page_with_tabs
permalink: /members/joon/projects/
tabs:
  - title: About
    url: /members/joon/
  - title: Projects
    url: /members/joon/projects/
  - title: Publications
    url: /members/joon/publications/
---

## Current Projects

##### Capacitive sensor array for in-situ measurement of frost & condensate accumulation on cold surfaces

Condensation frosting demonstration

<style>
  .paired-video-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
    margin: 1rem 0 1.5rem;
  }

  .paired-video-grid video {
    width: 100%;
    height: clamp(220px, 36vw, 460px);
    object-fit: contain;
    border-radius: 0;
    background: #000;
  }

  @media (max-width: 900px) {
    .paired-video-grid {
      grid-template-columns: 1fr;
    }

    .paired-video-grid video {
      height: auto;
    }
  }
</style>

<div class="paired-video-grid">
  <video id="condensation-video-reference" autoplay muted playsinline loop>
    <source src="{{ '/members/joon/media/20260320_condensation_test_rec_synced_60x_trimmed.mp4' | relative_url }}" type="video/mp4">
  </video>
  <video id="condensation-video-target" autoplay muted playsinline loop>
    <source src="{{ '/members/joon/media/20260320_condensation_test_synced_60x_trimmed.mp4' | relative_url }}" type="video/mp4">
  </video>
</div>

<script>
  (function () {
    const master = document.getElementById("condensation-video-reference");
    const slave = document.getElementById("condensation-video-target");

    if (!master || !slave) return;

    const DRIFT_TOLERANCE_SECONDS = 0.12;

    const syncToMaster = () => {
      if (!isFinite(master.currentTime)) return;
      if (Math.abs(master.currentTime - slave.currentTime) > DRIFT_TOLERANCE_SECONDS) {
        slave.currentTime = master.currentTime;
      }
    };

    const tryAutoplay = async () => {
      try {
        await Promise.all([master.play(), slave.play()]);
      } catch (e) {
        // Autoplay may fail in restrictive browser settings.
      }
    };

    master.addEventListener("play", () => {
      syncToMaster();
      slave.play().catch(() => {});
    });
    master.addEventListener("pause", () => slave.pause());
    master.addEventListener("seeking", syncToMaster);
    master.addEventListener("seeked", syncToMaster);
    master.addEventListener("ratechange", () => {
      slave.playbackRate = master.playbackRate;
    });
    master.addEventListener("volumechange", () => {
      slave.muted = master.muted;
      slave.volume = master.volume;
    });
    master.addEventListener("ended", () => {
      slave.currentTime = 0;
      slave.play().catch(() => {});
    });

    master.addEventListener("loadedmetadata", () => {
      slave.currentTime = master.currentTime || 0;
      tryAutoplay();
    });

    setInterval(() => {
      if (!master.paused && !slave.paused) syncToMaster();
    }, 250);
  })();
</script>

## Previous Projects
