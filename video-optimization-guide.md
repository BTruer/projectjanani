# Video Optimization Guide for Project Janani

## 🎬 Video Implementation Overview

Your Project Janani website now supports MP4 video backgrounds with scroll-based animations while maintaining fast load times through advanced optimization techniques.

## 📁 Video File Setup

### 1. Video File Naming
Place your video files in the project root or a `assets/videos/` folder:
```
project-janani/
├── your-video.mp4        # Main MP4 file
├── your-video.webm       # WebM version (optional, better compression)
├── your-poster-image.jpg # Poster image (fallback/loading state)
└── index.html
```

### 2. Update File References
In `index.html`, replace the placeholder file names:
```html
<video 
    id="heroVideo"
    class="hero-video"
    autoplay 
    muted 
    loop 
    playsinline
    preload="metadata"
    poster="your-poster-image.jpg"    <!-- Replace with your poster image -->
    data-src="ProjectJananiWebsiteClip.mp4"         <!-- Replace with your MP4 file -->
>
    <source data-src="ProjectJananiWebsiteClip.webm" type="video/webm">  <!-- Optional WebM -->
    <source data-src="ProjectJananiWebsiteClip.mp4" type="video/mp4">    <!-- Your MP4 file -->
</video>
```

## 🔧 Video Optimization Recommendations

### File Size & Quality
```bash
# Recommended video specs:
Duration: 10-30 seconds (looped)
Resolution: 1920x1080 (Full HD)
Frame Rate: 24-30 fps
Bitrate: 2-5 Mbps
File Size: Under 10MB (preferably 5MB or less)
```

### Compression Commands (using FFmpeg)
```bash
# Create optimized MP4
ffmpeg -i input.mov -c:v libx264 -preset slow -crf 22 -c:a aac -b:a 128k -movflags +faststart your-video.mp4

# Create WebM version (better compression)
ffmpeg -i input.mov -c:v libvpx-vp9 -crf 30 -b:v 2M -c:a libopus your-video.webm

# Extract poster image from video
ffmpeg -i your-video.mp4 -ss 00:00:01 -vframes 1 your-poster-image.jpg
```

### Advanced Optimization
```bash
# Ultra-compressed version for slow connections
ffmpeg -i input.mov -c:v libx264 -preset slow -crf 28 -vf "scale=1280:720" -r 24 -movflags +faststart your-video-compressed.mp4
```

## ⚡ Performance Features Implemented

### 1. Lazy Loading
- Video only loads when the hero section is in viewport
- Uses Intersection Observer for efficient detection

### 2. Adaptive Loading
- **Mobile devices**: Automatically uses fallback background image
- **Slow connections**: Detects 2G/slow-2G and uses fallback
- **Connection changes**: Dynamically adapts to network speed changes

### 3. Smart Playback Control
- **Visibility API**: Pauses video when tab is not active
- **Scroll-based**: Pauses when scrolled 80% out of view
- **Auto-resume**: Resumes when back in view

### 4. Multiple Format Support
- WebM for modern browsers (better compression)
- MP4 fallback for older browsers
- Automatic format selection

### 5. Progressive Enhancement
- Works without JavaScript (fallback background)
- Graceful degradation on video load errors
- Loading states with user feedback

## 🎨 Scroll Animation Effects

The video maintains the same scroll-based effects as the original:
- **Scale effect**: Video scales up as you scroll (1x to 1.5x)
- **Fade effect**: Video opacity decreases (100% to 30%)
- **Content animation**: Title moves up and fades out
- **Smooth transitions**: 60fps animations using requestAnimationFrame

## 📱 Mobile Optimization

### Automatic Fallbacks
- **Width ≤ 768px**: Uses background image instead of video
- **Slow connections**: Automatically switches to image
- **Battery optimization**: Reduces video processing on mobile

### Touch Considerations
- Video doesn't autoplay on some mobile browsers (iOS Safari)
- Poster image provides immediate visual feedback
- Touch-friendly scroll animations

## 🔍 Browser Support

| Feature | Chrome | Firefox | Safari | Edge | Mobile |
|---------|--------|---------|--------|------|--------|
| Video Background | ✅ | ✅ | ✅ | ✅ | ✅* |
| Scroll Animation | ✅ | ✅ | ✅ | ✅ | ✅ |
| Autoplay | ✅ | ✅ | ⚠️** | ✅ | ⚠️** |
| WebM Support | ✅ | ✅ | ❌ | ✅ | ✅ |

*Mobile uses fallback image  
**Requires user interaction for autoplay

## 🚀 Quick Start

1. **Prepare your video file**:
   ```bash
   ffmpeg -i your-raw-video.mov -c:v libx264 -preset slow -crf 22 -movflags +faststart hero-video.mp4
   ```

2. **Create poster image**:
   ```bash
   ffmpeg -i hero-video.mp4 -ss 00:00:01 -vframes 1 hero-poster.jpg
   ```

3. **Update HTML** (replace placeholders):
   ```html
   poster="hero-poster.jpg"
   data-src="hero-video.mp4"
   ```

4. **Test performance**:
   - Open DevTools → Network tab
   - Check video file size and load time
   - Test on mobile device
   - Verify smooth scrolling

## 📊 Performance Monitoring

### Key Metrics to Watch
- **Video file size**: < 10MB
- **Load time**: < 3 seconds on 3G
- **Scroll performance**: 60fps animations
- **Memory usage**: Monitor for video memory leaks

### Debug Console Messages
The implementation logs helpful information:
```javascript
// Success messages
"Video loaded successfully"

// Fallback scenarios
"Video failed to load, using fallback"
"Connection speed reduced, hiding video"
"Mobile device detected, using image fallback"
```

## 🎯 Best Practices

### Content Guidelines
1. **Duration**: Keep videos 10-30 seconds
2. **Motion**: Subtle, non-distracting movement
3. **Content**: Ensure it works without sound
4. **Loop point**: Seamless loop transition

### Technical Guidelines
1. **Test on slow connections**: Use Chrome DevTools throttling
2. **Monitor performance**: Check for frame drops
3. **Accessibility**: Always provide meaningful poster images
4. **SEO**: Video doesn't impact page indexing

### File Organization
```
assets/
├── videos/
│   ├── hero-video.mp4
│   ├── hero-video.webm
│   └── hero-compressed.mp4
└── images/
    └── hero-poster.jpg
```

## 🔧 Troubleshooting

### Common Issues

**Video not loading?**
- Check file path in `data-src`
- Verify video file isn't corrupted
- Check browser console for errors

**Poor performance?**
- Reduce video bitrate/resolution
- Enable hardware acceleration in browser
- Check if video is too long

**Autoplay blocked?**
- Ensure video has `muted` attribute
- Check browser autoplay policies
- Poster image will show as fallback

**Mobile issues?**
- This is expected behavior (uses image fallback)
- Test poster image quality
- Verify responsive design

## 📈 Advanced Customization

### Custom Animation Timing
```javascript
// Adjust scroll animation speed
const scale = 1 + (progress * 0.3); // Reduce scale effect
const opacity = 1 - (progress * 0.5); // Reduce fade effect
```

### Custom Loading States
```css
.video-loading {
    /* Customize loading appearance */
    background: rgba(0,0,0,0.8);
    border-radius: 8px;
    padding: 20px;
}
```

### Multiple Video Sources
```html
<!-- Add different quality options -->
<source data-src="hero-video-4k.mp4" type="video/mp4" media="(min-width: 1920px)">
<source data-src="hero-video-hd.mp4" type="video/mp4" media="(min-width: 1280px)">
<source data-src="hero-video-sd.mp4" type="video/mp4">
```

## 💡 Pro Tips

1. **Test early**: Video optimization can be time-consuming
2. **Monitor analytics**: Track bounce rates after video implementation
3. **A/B test**: Compare with image-only version
4. **CDN recommended**: Use video CDN for global distribution
5. **Backup plan**: Always have high-quality poster image ready

---

Your Project Janani website now features a professional, performant video background that enhances the storytelling while maintaining excellent user experience across all devices and connection speeds. 