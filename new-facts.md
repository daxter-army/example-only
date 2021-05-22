# 1. You dont need any external library/module to compress your images in js, can do it with one line of code.

# 2. WebRTC contains a wide number of experimental implementations, always see their compatibility chart, before implementing webrtc features in your web-app; second is caniuse.com; chrome supports the most.

# 3. navigator.mediaDevices.enumerateDevices() in chrome (or other chromium based browsers) will give you labels of connected media devices even without having camera permissions, but firefox will not.

# 4. typeof(null) => object

# 5. You cannot have two or more simultaneous running instances of navigator.mediaDevices.getUserMedia()

# 6. To erase a whole word, use Ctrl+Backspace/Delete

# 7. To clear terminal, use Ctrl+l