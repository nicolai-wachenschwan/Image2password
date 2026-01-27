# Image2password
How do you locally create true random Numbers on a Computer? Harvest randomness from your Environment. 

##  Security Philosophy

Why? Dont we use Libarys for that? Yes, but I like the idea of calculating something truely random on a computer, by harvesting the surrounding. Most "random" generators are purely mathematical (PRNGs). While usual approaches harvest mouse movements or some other scource of entropy **image2password** bridges the gap between software and physics by using your smartphone's camera as a **True Random Number Generator (TRNG)**.

### The Entropy Pool
1.  **image**: Every digital image contains "Shot Noise" and "Thermal Noise." Even if you restage a photo, the specific energy state of the sensor pixels at the nanosecond of capture is unique. We estimate ~1 bit of true, un-stageable entropy per pixel.
2.  **Salt (User Input)**: Provides a secondary entropy layer. If the salt length provides >513 bits of entropy (approx. 110 random letters), the tool allows image-free generation.
3.  **Temporal Noise**: Millisecond-precision timestamp to prevent identical state collisions. 



### SHA-512 Distillation
We collect megabytes of raw data and "distill" it using the **SHA-512** cryptographic hash. This ensures that:
* Even if an attacker recreates 99.9% of the photo, the **Avalanche Effect** of SHA-512 ensures the output is 100% different.
* The final 512-bit seed is uniformly distributed. Therefore only brute forcable. 

## The "Ghost Image" Risk
The biggest flaw in any image-based generator is the **source file persistence**. If the original image is saved to your gallery or cloud backup, the "True Randomness" becomes "Stored Data." To mitigate this to some extend we use the timestamp and the salt. 

**Protocol for High Security:**
* Immediately delete the photo after generation.
* Empty the "Recently Deleted" / "Trash" folder in your gallery.
* Disable cloud sync (iCloud/Google Photos) temporarily while using the tool.

## Developer Notes
* **Zero Dependencies**: Vanilla JavaScript using the Web Crypto API.
* **Modulo Fairness**: Uses `BigInt` to handle the 512-bit hash. Modulo bias is mathematically negligible ($< 2^{-400}$).
* **Client-Side Only**: No data ever leaves the device.

##  How to Use
Simply save the `.html` file to your device. Open it in any modern mobile browser. No internet connection is required after the initial load.

## Notes
Implementation was mostly done by Gemini 3, with a human touch here an there. 
