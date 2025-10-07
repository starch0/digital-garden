---
gists:
  - id: 11e6f74b9634bcc03d33ea77c4260967
    url: 'https://gist.github.com/starch0/11e6f74b9634bcc03d33ea77c4260967'
    createdAt: '2025-09-14T19:52:39Z'
    updatedAt: '2025-09-14T19:52:39Z'
    filename: Everything is number.md
    isPublic: true
    baseUrl: 'https://api.github.com'
---
## 1. Everything is number

In the realm of computing, machines don't perceive the world as we do. Instead, they rely on numerical abstractions—sequences of binary digits (0s and 1s) that represent everything from integers to complex algebraic structures. This fundamental axiom means that any real-world entity, whether a physical object or an abstract sensation, must be quantized and encoded into numbers for a computer to process it.

Take a digital photograph of Christ the Redeemer in Rio de Janeiro, Brazil. To the human eye, it’s a vivid image, but to a computer, it’s a grid of pixels, each represented as a triplet of integers in the RGB color space. For example, a foggy orange hue might be encoded as [255, 128, 64], where each number corresponds to the intensity of red, green, and blue on an 8-bit scale (0 to 255). Similarly, a GPS coordinate, like (37.7749, -122.4194) for latitude and longitude, transforms a physical location into a pair of floating-point numbers. These numerical representations enable computers to perform tasks like image enhancement or route optimization.
## 2. The real world as numbers

### Images
Images exemplify the discretization of visual reality into numerical arrays. An image is decomposed into a raster grid of pixels, where each pixel is a vector in a color space. In the RGB model, prevalent in digital imaging, each pixel is a 3-tuple of integers:

$p=(r,g,b),r,g,b∈[0,255]\mathbf{p} = (r, g, b), \quad r, g, b \in [0, 255]p=(r,g,b),r,g,b∈[0,255]$

for 24-bit color depth. For grayscale images, this simplifies to a scalar intensity value:

$i∈[0,255]i \in [0, 255]i∈[0,255]$

representing luminance.

This numerical foundation underpins advanced processing techniques. In facial recognition, algorithms like Eigenfaces or deep convolutional neural networks (CNNs) treat the image as a high-dimensional vector in:

$Rm×n×3\mathbb{R}^{m \times n \times 3}Rm×n×3$

(for an m×nm \times nm×n image). Feature extraction involves matrix operations, such as principal component analysis (PCA), where the covariance matrix is computed:

$Σ=1N∑i=1N(xi−μ)(xi−μ)T\Sigma = \frac{1}{N} \sum_{i=1}^N (\mathbf{x}_i - \mu)(\mathbf{x}_i - \mu)^TΣ=N1​i=1∑N​(xi​−μ)(xi​−μ)T$

over pixel vectors xi\mathbf{x}_ixi​, yielding eigenvectors that capture facial variances.

Image classification, as in models like ResNet, relies on backpropagation over these numerical tensors, minimizing loss functions like cross-entropy:
$$
L=−∑ylog⁡(y^)L = -\sum y \log(\hat{y})L=−∑ylog(y^​)
$$
where $yyy$ and $y^\hat{y}y^​$ are one-hot encoded labels and predictions derived from pixel values.
