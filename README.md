# Snooker Points Calculator

A lightweight, single-page web application that uses your device camera to analyze a snooker table and calculate the maximum remaining points according to official snooker rules.

## Features

- **Camera Integration**: Uses Web Camera API (getUserMedia) for mobile and desktop
- **Ball Detection**: Color-based heuristic detection system
- **Accurate Scoring**: Implements official snooker rules correctly
- **Clear Display**: Shows breakdown of remaining balls and point calculations
- **Lightweight**: Single HTML file, no build tools required
- **Mobile-First**: Responsive design optimized for all devices

## How to Use

1. Open `index.html` in a modern web browser
2. Click "Open Camera" and grant camera permissions
3. Point camera at the snooker table
4. Click "Capture Photo" when ready
5. View the calculated maximum remaining points and breakdown

## Ball Detection Approach

### Current Implementation (Heuristic Color Detection)

The current system uses **HSV color space analysis** to detect balls:

1. **Color Sampling**: Samples pixels from the captured image (every 5th pixel for performance)
2. **HSV Conversion**: Converts RGB pixels to HSV for better color matching
3. **Color Matching**: Checks each pixel against predefined HSV ranges for each ball color:
   - **Red**: Hue 0-20°, high saturation
   - **Yellow**: Hue 40-70°, high saturation
   - **Green**: Hue 70-150°, medium-high saturation
   - **Brown**: Hue 10-40°, lower saturation/value
   - **Blue**: Hue 180-250°, medium-high saturation
   - **Pink**: Hue 300-360°, lower saturation
   - **Black**: Very low value (brightness), low saturation
4. **Threshold Detection**: Counts matching pixels and uses thresholds to determine if a ball is present

### Limitations & Future Improvements

**Current Limitations:**
- May struggle with poor lighting conditions
- Can be affected by table color and background
- May misidentify balls if colors are similar
- Doesn't account for ball shadows or reflections
- No shape detection (circular blob detection)

**Recommended Improvements:**

1. **Machine Learning Approach**:
   - Train a TensorFlow.js model on snooker table images
   - Use object detection (YOLO, SSD) or semantic segmentation
   - Better accuracy across different lighting and angles

2. **Computer Vision Enhancements**:
   - Implement circular Hough transform for ball detection
   - Add edge detection to identify ball boundaries
   - Use perspective correction for angled table views
   - Implement blob detection to count individual balls

3. **Color Detection Improvements**:
   - Adaptive thresholding based on image lighting
   - Use LAB color space for better perceptual uniformity
   - Add color calibration step using known reference colors
   - Implement color clustering (K-means) to identify dominant colors

4. **Table Detection**:
   - Detect table boundaries first
   - Focus analysis on table surface only
   - Account for table felt color variations

## Snooker Rules Implementation

The calculator correctly implements official snooker scoring rules:

### Phase 1: While Reds Remain
- Each red ball = 1 point
- After potting a red, you can pot any colour
- Colours are respotted until all reds are gone
- **Maximum calculation**: `reds × (1 + highest_colour_value)`

### Phase 2: After All Reds Are Potted
- Colours must be potted in strict order:
  1. Yellow (2 points)
  2. Green (3 points)
  3. Brown (4 points)
  4. Blue (5 points)
  5. Pink (6 points)
  6. Black (7 points)
- Each colour is only potted once
- If a colour is missing, subsequent colours cannot be counted

### Example Calculation

**Scenario**: 3 reds, yellow, green, blue, pink, black remaining

1. **Reds phase**: 
   - 3 reds × 1 point = 3 points
   - 3 × black (7 points) = 21 points
   - Subtotal: 24 points

2. **Colours phase**:
   - Yellow (2) + Green (3) + Blue (5) + Pink (6) + Black (7) = 23 points

**Total Maximum**: 47 points

## Technical Details

### Dependencies
- **None** - Pure vanilla HTML, CSS, and JavaScript
- Uses only native browser APIs:
  - `getUserMedia` for camera access
  - Canvas API for image processing
  - No external libraries required

### Browser Compatibility
- Modern browsers with camera API support
- Chrome, Firefox, Safari, Edge (latest versions)
- Mobile browsers (iOS Safari, Chrome Mobile)

### Performance
- Lightweight: Single file, ~15KB
- Fast image analysis using pixel sampling
- No network requests after initial load

## Code Structure

The code is organized into clear sections:

1. **Camera & Image Capture**: Handles camera access and photo capture
2. **Ball Detection**: Color-based detection algorithms
3. **Snooker Rules Logic**: Scoring calculation engine
4. **Results Display**: UI for showing results

The detection logic is cleanly separated, making it easy to swap in improved algorithms (e.g., TensorFlow.js models) without changing other parts of the code.

## Assumptions Made

1. **Ball Detection**: Assumes balls are visible and not heavily obscured
2. **Table View**: Works best with a clear top-down or angled view of the table
3. **Lighting**: Assumes reasonable lighting conditions (not too dark or overexposed)
4. **Ball Count**: For reds, estimates count based on pixel density (may not be exact)
5. **Color Accuracy**: Assumes standard snooker ball colors (may vary with different ball sets)

## Future Enhancements

- [ ] TensorFlow.js model integration for better detection
- [ ] Manual ball count override option
- [ ] Save/load captured images
- [ ] History of calculations
- [ ] Different game modes (8-ball, 9-ball)
- [ ] Improved mobile camera controls
- [ ] Offline functionality with service worker

## License

Free to use and modify.

